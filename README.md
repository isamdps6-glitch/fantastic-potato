<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DayFlow — Day Optimizer</title>
<style>
:root {
  --bg: #0a0a12;
  --surface: #12121e;
  --surface2: #1a1a2e;
  --accent: #818cf8;
  --accent-dim: rgba(129,140,248,0.12);
  --accent-glow: rgba(129,140,248,0.28);
  --text: #e2e8f0;
  --muted: #64748b;
  --border: rgba(255,255,255,0.07);
  --radius: 12px;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:var(--bg);color:var(--text);height:100vh;display:flex;overflow:hidden;}

/* ── Sidebar ── */
.sidebar{width:272px;min-width:272px;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;padding:28px 22px;gap:0;}
.logo{display:flex;align-items:center;gap:10px;margin-bottom:36px;}
.logo-dot{width:30px;height:30px;background:var(--accent);border-radius:8px;display:grid;place-items:center;font-size:15px;flex-shrink:0;}
.logo-text{font-size:17px;font-weight:800;letter-spacing:-0.4px;}
.logo-sub{font-size:11px;color:var(--muted);margin-top:1px;}

.controls{display:flex;flex-direction:column;gap:18px;flex:1;}
.cg label{display:block;font-size:10.5px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:var(--muted);margin-bottom:7px;}
.cg select{
  width:100%;background:var(--surface2);border:1px solid var(--border);border-radius:9px;
  color:var(--text);font-size:13.5px;padding:10px 36px 10px 12px;cursor:pointer;outline:none;
  appearance:none;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath fill='%2364748b' d='M5 6L0 0h10z'/%3E%3C/svg%3E");
  background-repeat:no-repeat;background-position:right 13px center;
  transition:border-color .18s,box-shadow .18s;
}
.cg select:focus{border-color:var(--accent);box-shadow:0 0 0 3px var(--accent-dim);}
.cg select option{background:#1a1a2e;}

.btn{
  margin-top:24px;background:var(--accent);color:#fff;border:none;border-radius:var(--radius);
  padding:13px;font-size:13.5px;font-weight:800;letter-spacing:.2px;cursor:pointer;
  position:relative;overflow:hidden;transition:transform .15s,box-shadow .15s;
}
.btn:hover{transform:translateY(-2px);box-shadow:0 10px 30px var(--accent-glow);}
.btn:active{transform:translateY(0);}
.btn.shake{animation:shake .38s ease;}

/* ── Main ── */
.main{flex:1;overflow-y:auto;padding:40px 44px;display:flex;flex-direction:column;gap:28px;}
.main::-webkit-scrollbar{width:5px;}
.main::-webkit-scrollbar-thumb{background:var(--border);border-radius:3px;}

.pg-title{font-size:26px;font-weight:800;letter-spacing:-.5px;}
.pg-sub{font-size:13.5px;color:var(--muted);margin-top:5px;}

/* ── Empty state ── */
.empty{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;text-align:center;padding:60px;}
.empty-ring{width:72px;height:72px;border-radius:50%;border:2px solid var(--border);display:grid;place-items:center;font-size:28px;opacity:.35;margin-bottom:4px;}
.empty h3{font-size:17px;font-weight:700;opacity:.45;}
.empty p{font-size:13.5px;color:var(--muted);max-width:300px;line-height:1.65;}

/* ── Schedule ── */
.schedule{display:none;flex-direction:column;gap:20px;}
.schedule.on{display:flex;animation:fadeUp .35s ease;}

.tags{display:flex;gap:8px;flex-wrap:wrap;}
.tag{background:var(--accent-dim);color:var(--accent);border:1px solid rgba(129,140,248,.22);border-radius:20px;padding:4px 13px;font-size:11.5px;font-weight:700;}

.headline{font-size:20px;font-weight:700;letter-spacing:-.3px;line-height:1.4;color:var(--text);}

.steps{display:flex;flex-direction:column;gap:13px;}
.card{
  background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
  padding:22px 22px 22px 25px;display:flex;gap:18px;position:relative;overflow:hidden;
  transition:border-color .18s,box-shadow .18s;
  animation:fadeUp .4s ease both;
}
.card:nth-child(1){animation-delay:.04s;}
.card:nth-child(2){animation-delay:.1s;}
.card:nth-child(3){animation-delay:.16s;}
.card::before{content:'';position:absolute;left:0;top:0;bottom:0;width:3px;background:var(--accent);border-radius:3px 0 0 3px;}
.card:hover{border-color:rgba(129,140,248,.28);box-shadow:0 4px 22px rgba(0,0,0,.35);}

.num{width:34px;height:34px;min-width:34px;border-radius:50%;background:var(--accent-dim);border:1px solid rgba(129,140,248,.28);display:grid;place-items:center;font-size:13px;font-weight:800;color:var(--accent);}
.body{flex:1;}
.row{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:8px;}
.step-title{font-size:15px;font-weight:700;}
.dur{font-size:11.5px;font-weight:700;color:var(--accent);background:var(--accent-dim);padding:3px 11px;border-radius:12px;white-space:nowrap;}
.desc{font-size:13.5px;color:var(--muted);line-height:1.68;}
.tip{margin-top:10px;font-size:12px;color:var(--muted);background:var(--surface2);border-radius:7px;padding:8px 12px;border-left:2px solid var(--accent);line-height:1.6;}

.footer-bar{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:15px 22px;display:flex;align-items:center;justify-content:space-between;}
.footer-bar span:first-child{font-size:13px;color:var(--muted);}
.footer-bar span:last-child{font-size:15px;font-weight:800;color:var(--accent);}

/* ── Animations ── */
@keyframes fadeUp{from{opacity:0;transform:translateY(14px);}to{opacity:1;transform:translateY(0);}}
@keyframes shake{0%,100%{transform:translateX(0);}20%{transform:translateX(-7px);}40%{transform:translateX(7px);}60%{transform:translateX(-4px);}80%{transform:translateX(4px);}}
/* ── Chat Panel ── */
  .chat-fab{position:fixed;bottom:28px;right:28px;width:50px;height:50px;border-radius:50%;background:var(--accent);border:none;color:#fff;font-size:20px;cursor:pointer;box-shadow:0 6px 24px var(--accent-glow);transition:transform .15s,box-shadow .15s;z-index:100;}
  .chat-fab:hover{transform:scale(1.08);}

  .chat-panel{position:fixed;bottom:90px;right:28px;width:360px;height:490px;background:var(--surface);border:1px solid var(--border);border-radius:16px;display:flex;flex-direction:column;overflow:hidden;box-shadow:0 20px 60px rgba(0,0,0,.5);z-index:99;transform:scale(.93) translateY(14px);opacity:0;pointer-events:none;transition:transform .22s ease,opacity .22s ease;}
  .chat-panel.open{transform:scale(1) translateY(0);opacity:1;pointer-events:all;}

  .chat-hdr{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;border-bottom:1px solid var(--border);background:var(--surface2);}
  .chat-hdr-info{display:flex;align-items:center;gap:10px;}
  .chat-av{width:30px;height:30px;background:var(--accent);border-radius:8px;display:grid;place-items:center;font-size:13px;color:#fff;}
  .chat-name{font-size:13.5px;font-weight:700;}
  .chat-sub{font-size:11px;color:var(--muted);margin-top:1px;}
  .chat-x{background:none;border:none;color:var(--muted);font-size:16px;cursor:pointer;line-height:1;padding:4px;}
  .chat-x:hover{color:var(--text);}

  .chat-msgs{flex:1;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:10px;}
  .chat-msgs::-webkit-scrollbar{width:4px;}
  .chat-msgs::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px;}

  .bubble{max-width:86%;padding:10px 13px;border-radius:12px;font-size:13px;line-height:1.62;}
  .bubble.bot{background:var(--surface2);color:var(--text);align-self:flex-start;border-bottom-left-radius:4px;}
  .bubble.user{background:var(--accent);color:#fff;align-self:flex-end;border-bottom-right-radius:4px;}
  .bubble em{color:#a5b4fc;font-style:normal;}

  .chat-bar{display:flex;gap:8px;padding:12px;border-top:1px solid var(--border);}
  .chat-in{flex:1;background:var(--surface2);border:1px solid var(--border);border-radius:9px;color:var(--text);font-size:13px;padding:9px 12px;outline:none;transition:border-color .18s;}
  .chat-in:focus{border-color:var(--accent);}
  .chat-in::placeholder{color:var(--muted);}
  .chat-go{width:36px;height:36px;flex-shrink:0;border-radius:8px;background:var(--accent);border:none;color:#fff
  ;font-size:17px;cursor:pointer;transition:background .15s;}
  .chat-go:hover{background:#6366f1;}
</style>
</head>
<body>

<aside class="sidebar">
  <div class="logo">
    <div class="logo-dot">⚡</div>
    <div>
      <div class="logo-text">DayFlow</div>
      <div class="logo-sub">Eliminate choice paralysis</div>
    </div>
  </div>

  <div class="controls">
    <div class="cg">
      <label>Energy Level</label>
      <select id="energy">
        <option value="">Choose...</option>
        <option value="low">🔋 Low</option>
        <option value="medium">⚡ Medium</option>
        <option value="high">🔥 High</option>
      </select>
    </div>
    <div class="cg">
      <label>Available Time</label>
      <select id="time">
        <option value="">Choose...</option>
        <option value="30">⏱ 30 Minutes</option>
        <option value="60">🕐 1 Hour</option>
        <option value="120">🗓 2+ Hours</option>
      </select>
    </div>
    <div class="cg">
      <label>Primary Focus</label>
      <select id="focus">
        <option value="">Choose...</option>
        <option value="academics">📚 Academics</option>
        <option value="fitness">💪 Fitness</option>
        <option value="creative">🎨 Creative Hobby</option>
      </select>
    </div>
  </div>

  <button class="btn" id="optBtn" onclick="optimize()">✦ Optimize My Day</button>
</aside>

<main class="main">
  <div>
    <div class="pg-title">Your Optimized Schedule</div>
    <div class="pg-sub">Set your parameters and click optimize — your 3-step plan appears instantly.</div>
  </div>

  <div class="empty" id="empty">
    <div class="empty-ring">◎</div>
    <h3>Ready to build your day</h3>
    <p>Select your energy level, time block, and focus area to get a personalized action plan with no decisions left to make.</p>
  </div>

  <div class="schedule" id="schedule">
    <div class="tags" id="tags"></div>
    <div class="headline" id="headline"></div>
    <div class="steps" id="steps"></div>
    <div class="footer-bar">
      <span>Total session time</span>
      <span id="total"></span>
    </div>
  </div>
</main>

<script>
const S = {
  low: {
    30: {
      academics: {
        h: "Rest your mind — but keep the momentum alive.",
        t: "30 min",
        s: [
          { n: "Passive Re-Read", d: "10 min", b: "Flip through your notes or highlights from the last session without writing a thing. Let your eyes glide over the material. Recognition reinforces memory with almost zero cognitive cost.", i: "If your eyes start glazing, stop early. Forced reading is wasted time." },
          { n: "Concept Mind Map", d: "15 min", b: "Pick one main topic and sketch a loose mind map on paper — boxes, arrows, any layout. Connecting ideas cements them without heavy effort.", i: "Use colors if you have them. Visual anchors improve low-energy recall." },
          { n: "One-Sentence Summary", d: "5 min", b: "Write a single sentence describing what you reviewed. This micro-synthesis closes the loop and gives your next session a clean starting point.", i: "Leave it visible on your desk for tomorrow." }
        ]
      },
      fitness: {
        h: "Move gently — your body is asking for care, not performance.",
        t: "30 min",
        s: [
          { n: "Slow Joint Warm-Up", d: "5 min", b: "Neck rolls, shoulder circles, wrist rotations, ankle circles. Take 3 slow breaths between each. This isn't a workout — it's a signal to your body that you care.", i: "Do this seated if you need to. No standards today." },
          { n: "Easy 15-Minute Walk", d: "15 min", b: "Walk at whatever pace feels natural. Go outside if possible — natural light and fresh air do more for low-energy states than any exercise.", i: "Leave headphones at home. The mental quiet is part of the benefit." },
          { n: "Restorative Stretch", d: "10 min", b: "Child's pose, seated forward fold, supine twist. Hold each 60 seconds. Breathe into tightness. This is recovery, not cardio.", i: "Finishing this session is the win. Log it and be proud." }
        ]
      },
      creative: {
        h: "Low energy is perfect for consuming — let ideas find you.",
        t: "30 min",
        s: [
          { n: "Inspiration Browse", d: "10 min", b: "Look at work you admire — artists, writers, designers, musicians. No judgment, no comparison. Just absorb what makes you feel something.", i: "Bookmark anything that sparks even a flicker of interest." },
          { n: "Freeform Doodle", d: "15 min", b: "Sketch, write, or noodle on your instrument without any goal. Ugly is allowed. Weird is encouraged. Low-pressure creation keeps the habit alive.", i: "Set a timer and stop when it rings. Done means done." },
          { n: "Capture One Idea", d: "5 min", b: "Write down the single most interesting thing that surfaced — a phrase, a shape, a sound, a feeling. This seed is tomorrow's starting point.", i: "Put it in a dedicated ideas note or sketchbook, not a loose scrap." }
        ]
      }
    },
    60: {
      academics: {
        h: "Absorb, don't grind. Gentle input is still real progress.",
        t: "60 min",
        s: [
          { n: "Skim & Highlight", d: "20 min", b: "Read through assigned material at a comfortable pace. Mark anything interesting with a single underline. Skip anything that glazes your eyes — you can return next session.", i: "Passive exposure builds familiarity. You're priming your brain, not testing it." },
          { n: "Watch a Lecture", d: "30 min", b: "Find a YouTube explanation or recorded lecture on your topic. Set it to 1.25x. Let the concepts wash over you — you're building context, not taking notes.", i: "Pause only when something genuinely surprises you." },
          { n: "Bullet the Highlights", d: "10 min", b: "Write 3 bullet points — the most interesting things you encountered. No full sentences required. This cements short-term memory into something retrievable.", i: "Date these bullets. They become a valuable revision log over time." }
        ]
      },
      fitness: {
        h: "Slow is intentional. Recovery is training too.",
        t: "60 min",
        s: [
          { n: "Gentle Yoga Flow", d: "20 min", b: "Follow a slow-flow YouTube yoga session. Move at your own pace — skip anything that doesn't feel good in your body today.", i: "Use blocks or pillows for modifications. Comfort over correctness." },
          { n: "Mindful Walk", d: "25 min", b: "A slow walk with no destination. Leave your phone. Notice colors, sounds, textures. This is active meditation and more restorative than you'd expect.", i: "Low-energy days benefit enormously from nature exposure. Even a park bench counts." },
          { n: "Breathwork & Wind-Down", d: "15 min", b: "Legs up the wall for 5 minutes, then 4-7-8 breathing for 10 rounds. Your nervous system shifts into parasympathetic mode — that's real recovery.", i: "This is a legitimate training tool. Elite athletes use it." }
        ]
      },
      creative: {
        h: "Consume richly today. Creation feeds on what you take in.",
        t: "60 min",
        s: [
          { n: "Deep Consumption", d: "25 min", b: "Read a chapter, watch a scene, listen to a full album, or study a painting slowly. Don't rush. Sit with what you're experiencing without multitasking.", i: "Choose something slightly outside your usual style. Cross-pollination sparks new ideas." },
          { n: "Stream of Consciousness", d: "25 min", b: "Write, draw, or play with zero editing and zero judgment. Fill the page, canvas, or track. This isn't about output quality — it's about staying connected to your practice.", i: "Never delete stream-of-consciousness work. Revisit it in a week." },
          { n: "Curate One Artifact", d: "10 min", b: "Identify the one thing you made or experienced today that you'd actually keep. Save, bookmark, or screenshot it. Curating your own work is part of creative development.", i: "These micro-collections become mood boards and evidence you showed up." }
        ]
      }
    },
    120: {
      academics: {
        h: "A long, easy session. Volume over intensity today.",
        t: "2 hrs",
        s: [
          { n: "Lecture Marathon", d: "60 min", b: "Watch recorded lectures or video explanations. Two or three 20-minute videos work better than one long one. Pause at natural breaks. No notes required.", i: "Structure: 20 min video → 2 min stretch → repeat. Momentum without strain." },
          { n: "Note Reorganization", d: "45 min", b: "Re-type, reformat, or restructure your existing notes without adding new content. Color-code headers, add spacing, create consistent layouts. Low effort, lasting asset.", i: "Clean notes are easier to study from later. You're investing in future-you." },
          { n: "Easy Flashcard Review", d: "15 min", b: "Run through flashcards you already mostly know. This confidence-building review solidifies familiar material and ends the session on a win.", i: "Use spaced repetition apps like Anki to queue cards automatically." }
        ]
      },
      fitness: {
        h: "Two hours of gentle restoration. Pros call this active recovery.",
        t: "2 hrs",
        s: [
          { n: "Body Scan Meditation", d: "30 min", b: "Lie flat. Starting from your toes, slowly bring attention to each body part. Notice tension without forcing release. A 30-minute body scan resets the nervous system better than a nap.", i: "Use a guided body scan on YouTube if the silence feels too loud." },
          { n: "Yin Yoga Session", d: "50 min", b: "Hold deep, passive poses for 3-5 minutes each. Target hips, spine, and hamstrings. Yin works connective tissue and fascia that regular exercise often ignores.", i: "Breathing is the practice inside the practice. Exhale fully into each hold." },
          { n: "Slow Walk & Fresh Air", d: "40 min", b: "A long, aimless stroll. No pace goal, no route. This final movement flushes lactic acid, boosts mood via light exposure, and signals your body that rest is safe.", i: "Bring water. The only rule is no rushing." }
        ]
      },
      creative: {
        h: "Fill your creative reservoir. Intake is the foundation of output.",
        t: "2 hrs",
        s: [
          { n: "Deep Inspiration Dive", d: "50 min", b: "Pick a rabbit hole and fall in: an artist's full body of work, a music subgenre you've never explored, a design movement. Give it real time.", i: "Screenshot or note anything that genuinely excites you — no matter how small." },
          { n: "Ambient Creation", d: "60 min", b: "Put on an instrumental playlist and create with zero pressure. One medium, one page, one track. Let the music guide the mood. This is creative play, not production.", i: "Turn off notifications. The goal is time in, not output volume." },
          { n: "Curate & Collect", d: "10 min", b: "Review what you made and what inspired you. Save the best 3-5 things into a dedicated reference folder. These become seeds for your next high-energy session.", i: "Name folders by date: '2026-06 Inspiration.' You'll thank yourself in 3 months." }
        ]
      }
    }
  },

  medium: {
    30: {
      academics: {
        h: "Sharp, focused, efficient. Thirty minutes is enough to move the needle.",
        t: "30 min",
        s: [
          { n: "Quick Activation", d: "5 min", b: "Scan the last page of notes from your previous session. This primes working memory and creates a cognitive bridge — recall is dramatically improved with this one step.", i: "Don't review too much. You want to activate, not re-study." },
          { n: "Focused Problem Sprint", d: "20 min", b: "Set a timer and tackle 2-3 practice problems or drill a specific concept. Close every other tab. The constraint creates urgency that passive study never does.", i: "If you finish early, add another problem rather than checking your phone." },
          { n: "Gap Analysis", d: "5 min", b: "Review what you solved. Mark anything uncertain with a question mark. Write the next session's starting problem — this makes your next session frictionless.", i: "Gaps found now are gaps closed before the exam." }
        ]
      },
      fitness: {
        h: "Short and sharp. This session punches above its weight.",
        t: "30 min",
        s: [
          { n: "Dynamic Warm-Up", d: "5 min", b: "Jumping jacks x 30, leg swings x 10 each side, arm circles x 15, high knees x 20. Heart rate up, joints lubricated, in 5 minutes flat.", i: "Never skip this. Cold muscles and fast effort is how injuries happen." },
          { n: "20-Min HIIT or Run", d: "20 min", b: "Option A: 4 rounds of 40-sec work / 20-sec rest (squats, push-ups, mountain climbers, lunges, burpees). Option B: 2km tempo run. Push to 7/10 effort.", i: "Log your performance. Progress tracking is the most underrated fitness tool." },
          { n: "Static Cool-Down", d: "5 min", b: "Quad stretch, hamstring stretch, hip flexor lunge, shoulder cross-body. 45 seconds each. Your muscles are warm — this is your best window to build flexibility.", i: "Deep exhale on every stretch. Your nervous system releases tension with the breath." }
        ]
      },
      creative: {
        h: "A tight window forces creative focus. Use it.",
        t: "30 min",
        s: [
          { n: "Mood & Setup", d: "5 min", b: "Pick a playlist that matches the energy you want to create with. Clear your workspace. Open the right files or tools. A clean start is professional craft, not procrastination.", i: "Keep a 'creation' playlist ready so this takes 60 seconds, not 5 minutes." },
          { n: "Focused Sprint", d: "20 min", b: "Work on one specific element: one scene, one section, one layer, one verse. Constrain the scope deliberately — finishing something small beats starting something big.", i: "Silence notifications at the OS level. Your focus is worth protecting." },
          { n: "Handoff Note", d: "5 min", b: "Write: 'I was working on ___ and the next step is ___.' Leave it open on your screen. Future-you picks up with zero ramp-up time.", i: "Professionals use this trick to eliminate the 'where was I?' friction that kills momentum." }
        ]
      }
    },
    60: {
      academics: {
        h: "A full hour of active, engaged learning. This is how mastery builds.",
        t: "60 min",
        s: [
          { n: "Active Reading Block", d: "25 min", b: "Read with intent: annotate in margins, underline sparingly, and pause every page to ask 'what is this actually saying?' Slow, engaged reading beats fast passive reading every time.", i: "Write your questions as you read. Forming questions beats highlighting for retention." },
          { n: "Problem Set Sprint", d: "25 min", b: "Attempt 5-8 practice problems or an exam-style question set. No looking at solutions until you've genuinely tried. Productive struggle — even when wrong — is how skills form.", i: "3-minute limit per problem. If stuck, move on and return. Momentum matters." },
          { n: "Error Analysis", d: "10 min", b: "For every wrong answer, write the correct method once. Not the answer — the method. Understanding the process, not memorizing results, is what transfers to unseen questions.", i: "Keep an error log. Reviewing it the night before an exam is one of the highest-ROI study moves." }
        ]
      },
      fitness: {
        h: "A complete session. Warm up, build, cool down — the full arc.",
        t: "60 min",
        s: [
          { n: "Warm-Up & Activation", d: "10 min", b: "Foam roll the muscles you're training today (2 min), dynamic stretches (4 min), then activation exercises like glute bridges or band pull-aparts (4 min). Arrive at your main set fully ready.", i: "Your first work set should feel almost easy. If it doesn't, your warm-up was too short." },
          { n: "Main Workout Block", d: "40 min", b: "Strength session (3-4 compound movements, 3-4 sets each) or a 5km run or a full class. Push to 7-8/10 effort — challenging, but you could push harder if you had to.", i: "Rest 60-90 sec between sets for hypertrophy, 2-3 min for strength. Timers help." },
          { n: "Cool-Down & Log", d: "10 min", b: "5 minutes easy movement, then 5 minutes targeted stretching. Log your sets, reps, weight, or distance. Measurement creates accountability and reveals progress over time.", i: "The log is the system. Without data, you're guessing." }
        ]
      },
      creative: {
        h: "Real creative work happens in sessions like this. Show up fully.",
        t: "60 min",
        s: [
          { n: "Session Brief", d: "5 min", b: "Write today's goal in one sentence: what you will make, finish, or advance. Ambiguity is the enemy of creative sessions. Specificity creates a track to run on.", i: "Put the brief somewhere visible — on a sticky note or at the top of your file." },
          { n: "Deep Creation Block", d: "45 min", b: "Work on your project with full presence. Phone away, notifications off, one tab open. If you get stuck, write through the resistance rather than switching tasks — the block usually breaks.", i: "45 minutes of real work beats 3 hours of half-distracted effort every time." },
          { n: "Critical Review", d: "10 min", b: "Step back and experience what you made as an audience member. Note one thing working and one thing to improve next session. This dual awareness is a professional skill.", i: "Don't edit during review — just observe and note. Editing is a separate mode." }
        ]
      }
    },
    120: {
      academics: {
        h: "Two hours of intentional study. This is where understanding deepens.",
        t: "2 hrs",
        s: [
          { n: "Deep Study Blocks", d: "60 min", b: "Two Pomodoro cycles (25 on, 5 break, 25 on, 5 break). Tackle your most difficult topic first while your focus is sharpest. No multitasking. One concept at a time.", i: "Use the 5-minute breaks physically — stand, stretch, look out a window. Don't scroll." },
          { n: "Practice & Application", d: "45 min", b: "Problem sets, past exam papers, writing drafts, or coding exercises. This is active retrieval — the most evidence-backed study method in cognitive science. Struggle is the signal learning is happening.", i: "Mixed practice (varied problem types) is harder but builds stronger retention than blocked practice." },
          { n: "Wrap-Up & Forward Plan", d: "15 min", b: "Summarize today's session in 5 bullets. Write tomorrow's three priorities. Close your books deliberately — this ritualized ending helps your brain consolidate memory during sleep.", i: "The wrap-up is not optional. It's when short-term input becomes long-term knowledge." }
        ]
      },
      fitness: {
        h: "A full, comprehensive training session. Don't leave anything on the table.",
        t: "2 hrs",
        s: [
          { n: "Thorough Warm-Up", d: "15 min", b: "Foam rolling (5 min), dynamic mobility (5 min), movement-specific activation (5 min). If lifting, work up to your working weight with progressively heavier warm-up sets.", i: "Elite athletes spend 20-30% of training time warming up. It's not wasted time." },
          { n: "Main Training Block", d: "70 min", b: "Strength + accessory work, long run or ride, sport-specific practice, or a structured class. A longer session lets you train multiple qualities — strength and conditioning, or two movement patterns.", i: "If energy dips at 45 minutes, that's normal glycogen depletion. A small carb snack beforehand helps." },
          { n: "Recovery Protocol", d: "35 min", b: "10 min cool-down movement, 15 min full-body stretching (prioritize what you worked), contrast shower if available, and a protein-rich meal within 45 minutes of finishing.", i: "Adaptation happens during recovery, not during training. This phase is as important as the workout itself." }
        ]
      },
      creative: {
        h: "This is your most powerful creative window. Use it with intention.",
        t: "2 hrs",
        s: [
          { n: "Setup & Intention", d: "10 min", b: "Review your last session's handoff note. Set today's single specific goal in writing. Gather all resources before you start — mid-session interruptions to find files break flow states.", i: "A 10-minute setup prevents 30 minutes of scattered, half-productive work." },
          { n: "Deep Creative Work", d: "90 min", b: "Phone in another room. One application open. This is your longest uninterrupted block. Start with your hardest or most interesting problem. Flow state typically kicks in after 15-20 minutes — push through to get there.", i: "If you hit a wall at 45 minutes, do 5 box breaths and return. Don't quit." },
          { n: "Edit & Reflect", d: "20 min", b: "View what you created with fresh critical eyes. What's working? What needs work? Write 3 specific notes for next session, then close everything deliberately — the session is complete.", i: "The best creators are rigorous editors. Critiquing your own work is a skill to develop." }
        ]
      }
    }
  },

  high: {
    30: {
      academics: {
        h: "You're sharp. Attack the hardest material you have.",
        t: "30 min",
        s: [
          { n: "Target the Boss Level", d: "3 min", b: "Right now, identify the single hardest concept or problem you've been avoiding. Write it at the top of your page. Clarity of intention is half the battle.", i: "High energy is rare. Don't waste it on easy material — save easy for low-energy days." },
          { n: "Full Attack Mode", d: "22 min", b: "Work through the hardest problem without hints, without Googling. Struggle completely before looking anything up. This struggle is the actual learning — not a sign you're doing it wrong.", i: "Set a firm timer. When it rings, you stop regardless. Urgency sharpens focus." },
          { n: "Debrief & Document", d: "5 min", b: "Crack it? Write out your method for later review. Stuck? Write exactly where you got confused — this is more valuable than the solution. Confusion mapped is confusion conquered.", i: "Your confusion log is your most precise study guide." }
        ]
      },
      fitness: {
        h: "Full power output. No holding back today.",
        t: "30 min",
        s: [
          { n: "Explosive Primer", d: "5 min", b: "High knees x 30 sec, broad jumps x 5, burpees x 5, jump squats x 10. This activates fast-twitch fibers and signals your nervous system to fire at capacity.", i: "Go genuinely fast. Slow explosive warm-ups don't prepare you for high-intensity work." },
          { n: "Max Intensity Circuit", d: "20 min", b: "Tabata protocol: 8 rounds of 20-sec max effort / 10-sec rest across 4-5 exercises. Go at 9/10 effort — if you could talk in full sentences, you're not working hard enough.", i: "Tabata was developed for Olympic speed skaters. 4 minutes per exercise is devastating. Respect it." },
          { n: "Controlled Descent", d: "5 min", b: "Walk immediately — don't sit. Slow breathing: 4-count inhales, 8-count exhales. Your heart rate should drop 20+ BPM in these 5 minutes. Honor the landing as much as the flight.", i: "The faster you recover between sessions, the faster your fitness improves. Cooldowns build that capacity." }
        ]
      },
      creative: {
        h: "Channel this energy into something bold. Strike while the iron's hot.",
        t: "30 min",
        s: [
          { n: "Rapid Prototype", d: "5 min", b: "Write, sketch, or record your boldest creative idea right now — the one you've been second-guessing. Don't refine, don't filter. Get the raw shape onto the page in 5 minutes.", i: "Bold ideas feel risky in calm moments. High energy removes that friction. Use it." },
          { n: "Build at Speed", d: "20 min", b: "Create a rough first version as fast as possible. Speed over polish, quantity over quality, output over perfection. A rough draft exists. A perfect plan doesn't. Existence wins.", i: "Set the timer. When it rings, what you have is the draft. It's enough." },
          { n: "Capture Momentum", d: "5 min", b: "Write the next 3 specific actions for this project. Not goals — actions. 'Write the bridge.' 'Finalize the palette.' 'Draft the next paragraph.' Momentum preserved is momentum compounded.", i: "You will not feel this energy again for a while. Write future-you a map while the trail is clear." }
        ]
      }
    },
    60: {
      academics: {
        h: "An hour of high-focus intensity. Tackle what's hardest and own it.",
        t: "60 min",
        s: [
          { n: "Choose Your Hardest Problem", d: "10 min", b: "Review your problem set or syllabus to identify the most challenging material. Rank your uncertainty. Commit to the #1 hardest item. Write it at the top of your page.", i: "This deliberate selection prevents you from defaulting to comfortable, easy review." },
          { n: "Deep Problem-Solving", d: "40 min", b: "Work through the problem completely. No hints for the first 20 minutes — genuine struggle is non-negotiable. After 20 minutes, allow one reference if truly stuck. Finish before the timer ends.", i: "Use scratch paper liberally. Getting ideas out of your head accelerates solving." },
          { n: "Error Analysis", d: "10 min", b: "Grade your work ruthlessly. For every wrong step, write the correct approach in full. Active error correction — not passive re-reading — is the highest-ROI study activity.", i: "Photograph your error analysis notes. Review them the night before the exam." }
        ]
      },
      fitness: {
        h: "A complete performance session. Every system firing.",
        t: "60 min",
        s: [
          { n: "Performance Warm-Up", d: "10 min", b: "Foam roll (2 min), dynamic mobility circuit (4 min), plyometric activation — box jumps, broad jumps (2 min), build-up sets to working weight (2 min). You should be sweating before your first work set.", i: "Your warm-up should feel like the beginning of training, not a delay before training." },
          { n: "Peak Session", d: "40 min", b: "Go for a personal record, run your fastest pace on a measured route, attempt a skill you've been building toward, or complete a challenging class at full effort. This is what high-energy days are built for.", i: "Log every rep, set, distance, and time. PRs only count when documented." },
          { n: "Active Recovery & Log", d: "10 min", b: "Light movement (walk or easy cycle), targeted stretching for what you worked, then 5 minutes logging your performance data. Note how you felt at peak — this calibrates future sessions.", i: "The best training logs include a one-line 'how it felt' note. It becomes invaluable over months." }
        ]
      },
      creative: {
        h: "One hour, maximum creative output. Go for the risky idea.",
        t: "60 min",
        s: [
          { n: "Write the Vision", d: "5 min", b: "In one paragraph, describe the best version of what you want to create this session. Be specific and bold. This written intention acts as a north star when you get deep in the work and lose perspective.", i: "Re-read it at the 30-minute mark when focus naturally starts to waver." },
          { n: "Intense Creation Sprint", d: "45 min", b: "Make something more ambitious than you think you can finish. Aim higher than comfort. High-energy sessions are the time to stretch your creative reach — conservatism is for low-energy days.", i: "When you hit resistance at 25 minutes, push through. That's the threshold where breakthroughs happen." },
          { n: "Document & Expand", d: "10 min", b: "Review what you made. Write 5 bullets on what's working and 5 on what to push further. These notes capture a creator's perspective at peak clarity — they're worth more than they look.", i: "Share what you made with one person today. External feedback closes creative loops." }
        ]
      }
    },
    120: {
      academics: {
        h: "The deepest possible study session. This is how top students study.",
        t: "2 hrs",
        s: [
          { n: "Three-Pomodoro Sprint", d: "80 min", b: "25 min focused → 5 min break → 25 min → 5 min → 25 min → 5 min rest. Start with your hardest material while focus is peak. During breaks: stand, drink water, look away from screens. No phone.", i: "Use Pomodoros to track daily totals. 4+ Pomodoros = elite-level study output." },
          { n: "Teaching Mode", d: "30 min", b: "Explain today's most complex topic aloud, as if teaching someone who knows nothing. Record yourself if possible. Gaps in your explanation reveal gaps in your understanding far more reliably than re-reading.", i: "The Feynman technique: if you can't explain it simply, you don't understand it yet." },
          { n: "Synthesis & Forward Planning", d: "10 min", b: "Write a 5-point summary of everything you studied today. Then write your next 3 study session priorities. End with your #1 remaining area of uncertainty — this is where tomorrow begins.", i: "This planning step takes 10 minutes and saves 30. Highest-ROI thing you can do at session's end." }
        ]
      },
      fitness: {
        h: "An epic session. Volume, intensity, recovery — the full performance loop.",
        t: "2 hrs",
        s: [
          { n: "Ritual Warm-Up", d: "20 min", b: "Foam roll every major muscle group (8 min), dynamic mobility circuit (7 min), plyometric activation and build-up sets (5 min). Treat this as the beginning of the performance, not a delay before it.", i: "On high-energy days, most people skip warm-up because they feel good. This is exactly when it matters most." },
          { n: "Peak Performance Block", d: "80 min", b: "Personal record attempts on primary lifts, a long endurance effort (10km+ run), competitive sport practice, or a major cross-training session. Leave everything on the field.", i: "Break it into phases: ramp-up (15 min), peak output (50 min), maintenance (15 min). Pacing is a skill." },
          { n: "Elite Recovery Protocol", d: "20 min", b: "10 min cool-down walk, 10 min full-body mobility, cold or contrast shower, high-protein meal within 45 minutes, and 8+ hours of sleep tonight. Recovery is not optional — it's where adaptation happens.", i: "Log everything. In 3 months, this session's data shows you exactly how far you've come." }
        ]
      },
      creative: {
        h: "Your maximum creative output session. Build something you're proud of.",
        t: "2 hrs",
        s: [
          { n: "Set an Ambitious Goal", d: "10 min", b: "Write the most exciting possible outcome for this session — the version that would feel genuinely impressive to finish. Break it into 3 milestones. You may not hit all three. Aim high anyway.", i: "Ambitious goals on high-energy days produce more than comfortable goals on any day." },
          { n: "Full Flow State Session", d: "100 min", b: "Phone in another room. All notifications off. Work in 25-minute blocks with 5-minute physical breaks (no screens). This is your deepest possible creative session — protect it at all costs.", i: "If interrupted mid-flow, write one sentence about where you are before responding. Re-entering flow from a written anchor takes 5 minutes, not 20." },
          { n: "Curate & Share", d: "10 min", b: "Identify the single best thing you made. Refine it by 10% — one small polish pass. Then share it: post it, send it to someone, or save it in a 'finished work' folder. Completion is a practice.", i: "Finishing and sharing is where creative confidence comes from. The work has to leave the notebook." }
        ]
      }
    }
  }
};

const L = {
  energy: { low:"Low Energy", medium:"Medium Energy", high:"High Energy" },
  time:   { 30:"30 Minutes", 60:"1 Hour", 120:"2+ Hours" },
  focus:  { academics:"Academics", fitness:"Fitness", creative:"Creative Hobby" }
};

function optimize() {
  const e = document.getElementById('energy').value;
  const t = document.getElementById('time').value;
  const f = document.getElementById('focus').value;

  if (!e || !t || !f) {
    const btn = document.getElementById('optBtn');
    btn.classList.remove('shake');
    void btn.offsetWidth;
    btn.classList.add('shake');
    btn.addEventListener('animationend', () => btn.classList.remove('shake'), { once: true });
    if (!e) document.getElementById('energy').focus();
    else if (!t) document.getElementById('time').focus();
    else document.getElementById('focus').focus();
    return;
  }

  const data = S[e][t][f];
  document.getElementById('empty').style.display = 'none';

  const sc = document.getElementById('schedule');
  sc.classList.remove('on');
  void sc.offsetWidth;
  sc.classList.add('on');

  document.getElementById('tags').innerHTML =
    `<span class="tag">${L.energy[e]}</span><span class="tag">${L.time[t]}</span><span class="tag">${L.focus[f]}</span>`;

  document.getElementById('headline').textContent = data.h;
  document.getElementById('total').textContent = data.t;

  document.getElementById('steps').innerHTML = data.s.map((s, i) => `
    <div class="card">
      <div class="num">${i + 1}</div>
      <div class="body">
        <div class="row">
          <div class="step-title">${s.n}</div>
          <div class="dur">${s.d}</div>
        </div>
        <div class="desc">${s.b}</div>
        <div class="tip">💡 ${s.i}</div>
      </div>
    </div>
  `).join('');
}
function toggleChat() {
    const p = document.getElementById('chatPanel');
    p.classList.toggle('open');
    if (p.classList.contains('open')) document.getElementById('chatIn').focus();
  }

  function sendChat() {
    const input = document.getElementById('chatIn');
    const msg = input.value.trim();
    if (!msg) return;
    addBubble(msg, 'user');
    input.value = '';
    setTimeout(() => addBubble(botReply(msg), 'bot'), 380);
  }

  function addBubble(text, who) {
    const el = document.createElement('div');
    el.className = `bubble ${who}`;
    el.innerHTML = text;
    const box = document.getElementById('chatMsgs');
    box.appendChild(el);
    box.scrollTop = box.scrollHeight;
  }

  function botReply(msg) {
    const m = msg.toLowerCase();
    const energy = document.getElementById('energy').value;
    const time   = document.getElementById('time').value;
    const focus  = document.getElementById('focus').value;
    const active = document.getElementById('schedule').classList.contains('on');

    const eL = { low:'low', medium:'medium', high:'high' }[energy] || '—';
    const tL = { 30:'30 minutes', 60:'1 hour', 120:'2+ hours' }[time] || '—';
    const fL = { academics:'academics', fitness:'fitness', creative:'creative' }[focus] || '—';

    if (!active) {
      if (/^(hi|hey|hello)\b/.test(m))
        return "Hey! Set your parameters in the sidebar and click <em>Optimize My Day</em> first — then I can answer anything about your plan.";
      return "Generate a schedule first using the sidebar dropdowns, then come back and ask me anything about it.";
    }

    if (/^(hi|hey|hello|sup)\b/.test(m))
      return `Hey! You've got a <em>${eL} energy · ${tL} · ${fL}</em> session loaded. What do you want to know?`;

    if (m.includes('why') && /order|sequence|first|step 1/.test(m))
      return `The steps follow a <em>ramp → peak → close</em> arc. Step 1 warms up without burning resources.
  Step 2 is peak-value work when focus is sharpest. Step 3 locks in the session and seeds the next one.`;

    if (/step\s*1|first step/.test(m))
      return `Step 1 is your <em>entry ramp</em>. For a ${eL}-energy ${fL} session, jumping straight into peak
  effort causes friction and early fatigue. The ramp makes step 2 feel effortless by comparison.`;

    if (/step\s*2|second|middle/.test(m))
      return `Step 2 is the <em>core of your session</em> — highest-value work placed exactly when your warm-up
  has activated focus but fatigue hasn't arrived yet. It's the step not to cut short.`;

    if (/step\s*3|third|last|final/.test(m))
      return `Step 3 is your <em>close and capture</em>. Skipping it means losing 20-30% of what you processed.
  It also writes the starting point for your next session so you don't waste time re-orienting.`;

    if (/less time|only 15|10 min|no time|shorter/.test(m))
      return `Compressed version: skip step 1, run step 2 for 10 minutes, then do a 5-minute step 3. <em>Always
  keep step 3</em> — it's what makes short sessions feel complete rather than abandoned.`;

    if (/more time|extra|longer/.test(m))
      return `Extend <em>step 2</em> — that's where the compounding happens. Add a second round or a second
  topic. Don't stretch step 3 much; brief and sharp is the point.`;

    if (/alternative|different|instead|swap|replace/.test(m)) {
      const alts = {
        academics: `Academics swap: replace any reading with a YouTube explanation of the same topic, or use the
  Cornell method instead of free-form notes. Same time, different format.`,
        fitness:   `Fitness swap: replace HIIT with a brisk walk if joints are tired, or sub swimming/cycling for
  low-impact. Match the duration, not the specific exercise.`,
        creative:  `Creative swap: if you're blocked on your main project, spend 15 minutes on a totally
  unrelated micro-project. Constraint and novelty both unlock ideas.`
      };
      return alts[focus] || `Swap the step for a different format of the same activity — same duration, different
  delivery method.`;
    }

    if (/tired|exhaust|low energy|motivat|don.?t want|lazy|procrastinat/.test(m))
      return `Motivation follows action — it doesn't precede it. <em>Do just step 1</em>, nothing more. The
  session almost always carries itself from there. The bar is showing up, not performing.`;

    if (/tip|advice|hack|trick/.test(m)) {
      const tips = {
        academics: `End a study session <em>mid-problem on purpose</em>. The Zeigarnik effect means your brain
  keeps processing unfinished tasks subconsciously. You will return sharper.`,
        fitness:   `The <em>2-minute rule</em>: commit to only 2 minutes of the workout when you want to skip.
  Starting is the hardest part. You almost always continue.`,
        creative:  `Keep a <em>bad ideas file</em>. Write every idea including the embarrassing ones.
  Externalizing clears space, and bad ideas are often one pivot away from good ones.`
      };
      return tips[focus] || `Consistency across sessions beats any single-session optimization. Show up, do the
  thing, close the loop.`;
    }

    if (/tomorrow|next session|next time/.test(m))
      return `Next session: if today felt right, try one level up on time block. If it was a grind, drop one
  energy tier. The dropdowns are meant to be re-evaluated every session — your state changes daily.`;

    if (/decision fatigue|how does this work|what is this/.test(m))
      return `Decision fatigue is mental exhaustion from making too many choices. DayFlow eliminates it by
  <em>deciding for you</em> based on your current state — so your willpower goes into doing, not choosing.`;

    if (/done|finished|completed|all done/.test(m))
      return `Solid. Log it — even a one-line note. When you're ready for the next session, re-evaluate the
  dropdowns fresh. Consistency compounds.`;

    if (/thank|thx|ty\b/.test(m))
      return "Happy to help. Go make the session count. ✦";

    return `Good question. The core principle here is matching your <em>${eL} energy</em> to the right intensity
  of ${fL} work over <em>${tL}</em>. Which specific step or part do you want me to break down?`;
  }

  
</script>
 <!-- ── Chat Panel ── -->
  <button class="chat-fab" id="chatFab" onclick="toggleChat()" title="DayFlow Assistant">💬</button>

  <div class="chat-panel" id="chatPanel">
    <div class="chat-hdr">
      <div class="chat-hdr-info">
        <div class="chat-av">✦</div>
        <div>
          <div class="chat-name">DayFlow Assistant</div>
          <div class="chat-sub">Ask anything about your schedule</div>
        </div>
      </div>
      <button class="chat-x" onclick="toggleChat()">✕</button>
    </div>
    <div class="chat-msgs" id="chatMsgs">
      <div class="bubble bot">
        Hey! Generate a schedule first, then ask me things like <em>"why this order?"</em>, <em>"give me an
  alternative for step 2"</em>, or <em>"what if I only have 15 minutes?"</em>
      </div>
    </div>
    <div class="chat-bar">
      <input class="chat-in" id="chatIn" type="text" placeholder="Ask about your schedule..."
        onkeydown="if(event.key==='Enter')sendChat()" />
      <button class="chat-go" onclick="sendChat()">↑</button>
    </div>
  </div>
</body>
</html>
