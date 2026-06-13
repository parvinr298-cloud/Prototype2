<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SideQuest x NSU Cybernauts | Premium Ecosystem</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- React & ReactDOM -->
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  
  <!-- Framer Motion -->
  <script src="https://unpkg.com/framer-motion@10.12.16/dist/framer-motion.js"></script>
  
  <!-- Babel -->
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            obsidian: '#020408',
            surface: '#0a0f1a',
            card: 'rgba(15, 22, 36, 0.75)',
            brand: '#ff7300',
            brandHover: '#ff8c33',
            cyberBlue: '#00f0ff',
            success: '#10b981',
          },
          fontFamily: { sans: ['Inter', 'sans-serif'], mono: ['Fira Code', 'monospace'] },
          backgroundImage: {
            'grid-pattern': "linear-gradient(to right, rgba(255,255,255,0.02) 1px, transparent 1px), linear-gradient(to bottom, rgba(255,255,255,0.02) 1px, transparent 1px)"
          }
        }
      }
    }
  </script>

  <style>
    @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;700&family=Inter:wght@400;600;800;900&display=swap');
    body { background-color: #020408; color: white; overflow: hidden; margin: 0; padding: 0; }
    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: #020408; }
    ::-webkit-scrollbar-thumb { background: rgba(255, 115, 0, 0.5); border-radius: 10px; }
    .glass-panel { background: rgba(10, 15, 26, 0.7); backdrop-filter: blur(16px); border: 1px solid rgba(255,255,255,0.05); }
    .neon-text { text-shadow: 0 0 10px rgba(255,115,0,0.5), 0 0 20px rgba(255,115,0,0.3); }
    .cyber-border { position: relative; }
    .cyber-border::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: linear-gradient(90deg, transparent, var(--brand), transparent); opacity: 0.5; }
  </style>
</head>
<body class="bg-grid-pattern" style="background-size: 40px 40px;">

  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect } = React;
    const { motion, AnimatePresence } = window.Motion;

    // --- MOCK DATABASE ---
    const questsData = [
      {
        id: 1,
        employer: "NSU Cybernauts Core",
        employerImg: "https://images.unsplash.com/photo-1526304640581-d334cdbbf45e?auto=format&fit=crop&w=150&q=80",
        title: "Penetration Tester for CTF Arena",
        category: "Cybernauts 2026",
        pay: "à§³ 15,000 + VIP Pass",
        location: "NSU Main Campus",
        skills: ["Kali Linux", "Ethical Hacking", "Python"],
        shortDesc: "Help us test the vulnerabilities of the Cybernauts CTF platform before the national event.",
        fullDesc: "We need an experienced cybersecurity student to stress-test our Capture The Flag (CTF) servers. You will be actively trying to break our architecture to ensure it holds up when BUET and BRAC teams attack it during the main event.",
        points: ["Identify SQLi and XSS vulnerabilities", "Submit a formal bug bounty report", "Must sign an NDA"],
        linkText: "Accept Cybernauts Contract",
        status: "High Priority",
        xpReward: "+1500 XP"
      },
      {
        id: 2,
        employer: "TechCorp BD",
        employerImg: "https://images.unsplash.com/photo-1542744173-8e7e53415bb0?auto=format&fit=crop&w=150&q=80",
        title: "React Developer (Internship)",
        category: "Corporate Placements",
        pay: "à§³ 12,000 / month",
        location: "Remote / Banani",
        skills: ["React", "TypeScript", "Tailwind"],
        shortDesc: "Join our frontend team to build fintech dashboards for local banks.",
        fullDesc: "This is a 3-month paid internship with a high probability of a full-time offer. You will be working directly under our Senior Engineers building React components. Payment is secured via the SideQuest monthly escrow pipeline.",
        points: ["20 hours per week", "Daily standups via Google Meet", "Access to company GitHub"],
        linkText: "Submit Verified Portfolio",
        status: "Escrow Secured",
        xpReward: "+3000 XP"
      },
      {
        id: 3,
        employer: "Bean & Byte Cafe",
        employerImg: "https://images.unsplash.com/photo-1555396273-367ea4eb4db5?auto=format&fit=crop&w=150&q=80",
        title: "Social Media Manager",
        category: "Quick Gigs",
        pay: "à§³ 5,000 (Flat)",
        location: "Bashundhara R/A",
        skills: ["Marketing", "Canva", "TikTok"],
        shortDesc: "Manage our Instagram and TikTok for launch week.",
        fullDesc: "We need a creative NSU student to run our social media for our grand opening near Apollo Gate. Create 5 reels and 10 posts. Funds are already locked in bKash via SideQuest.",
        points: ["Must have smartphone with good camera", "Content calendar creation", "Engage with local student accounts"],
        linkText: "Accept Gig & Lock Funds",
        status: "Escrow Secured",
        xpReward: "+400 XP"
      }
    ];

    // --- COMPONENTS ---

    // 1. Cybernauts Boot Sequence
    const BootScreen = ({ onComplete }) => {
      return (
        <motion.div 
          initial={{ opacity: 1 }} animate={{ opacity: 0 }} transition={{ delay: 3, duration: 1 }}
          onAnimationComplete={onComplete}
          className="fixed inset-0 z-[100] bg-obsidian flex flex-col items-center justify-center font-mono"
        >
          <motion.div initial={{ scale: 0.9, opacity: 0 }} animate={{ scale: 1, opacity: 1 }} transition={{ duration: 0.5 }} className="text-center w-full max-w-2xl px-8">
            <h1 className="text-5xl font-black mb-2 text-white">SYSTEM<span className="text-brand">_BOOT</span></h1>
            <p className="text-cyberBlue mb-8 tracking-widest uppercase text-sm">NSU Cybernauts 2026 Edition</p>
            
            <div className="text-left text-xs text-gray-500 space-y-2 mb-8 h-32 overflow-hidden">
              <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.5 }}>[OK] Loading SideQuest Core Kernel...</motion.p>
              <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 1.0 }}>[OK] Establishing Secure bKash Escrow Pipeline...</motion.p>
              <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 1.5 }}>[OK] Syncing NSU Student Database...</motion.p>
              <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 2.0 }} className="text-brand">[WARN] Unprecedented Talent Levels Detected.</motion.p>
              <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 2.5 }} className="text-success">[OK] Ecosystem Ready. Handing over control.</motion.p>
            </div>

            <div className="h-1 bg-gray-900 w-full rounded-full overflow-hidden border border-gray-800">
              <motion.div initial={{ width: "0%" }} animate={{ width: "100%" }} transition={{ duration: 2.5, ease: "easeInOut" }} className="h-full bg-brand shadow-[0_0_15px_#ff7300]" />
            </div>
          </motion.div>
        </motion.div>
      );
    };

    // 2. The Portfolio View (The CV Killer)
    const PortfolioView = () => (
      <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} className="p-8 max-w-5xl mx-auto space-y-8">
        <div className="glass-panel p-8 rounded-2xl flex flex-col md:flex-row gap-8 items-center cyber-border">
          <div className="relative">
<img 
              src="https://i.ibb.co/cKwJ2pWB/FB-IMG-1781330470425.jpg"
              className="w-32 h-32 rounded-full border-4 border-brand object-cover shadow-[0_0_20px_rgba(255,115,0,0.4)]"
            />
            <div className="absolute -bottom-2 -right-2 bg-brand text-white text-xs font-black px-3 py-1 rounded-full border-2 border-obsidian">LVL 12</div>
          </div>
          <div className="flex-1 text-center md:text-left">
            <h2 className="text-3xl font-black text-white">Shafin Chowdhury</h2>
            <p className="text-brand font-mono text-sm mb-4">Govt. Tolaram University â€¢ B.Sc in CSE</p>
            <p className="text-gray-400 text-sm max-w-xl">Full-stack developer specializing in React and Node.js. Passionate about building fintech solutions. SideQuest verified Top 5% earner.</p>
          </div>
          <div className="flex gap-4">
            <div className="text-center p-4 bg-obsidian rounded-xl border border-gray-800">
              <p className="text-xs text-gray-500 uppercase font-bold">Quests Done</p>
              <p className="text-2xl font-black text-white">14</p>
            </div>
            <div className="text-center p-4 bg-obsidian rounded-xl border border-gray-800">
              <p className="text-xs text-gray-500 uppercase font-bold">Rating</p>
              <p className="text-2xl font-black text-success">4.9/5</p>
            </div>
          </div>
        </div>

        <h3 className="text-xl font-bold uppercase tracking-widest text-gray-300">Verified On-Chain Experience</h3>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div className="glass-panel p-6 rounded-xl border-l-4 border-brand hover:-translate-y-1 transition-transform">
            <div className="flex justify-between items-start mb-2">
              <h4 className="font-bold text-white text-lg">Frontend UI Engineer</h4>
              <span className="text-xs bg-success/20 text-success px-2 py-1 rounded font-bold">Verified Employer</span>
            </div>
            <p className="text-gray-400 text-sm mb-4">Built the main dashboard for a local logistics startup using React and Tailwind.</p>
            <p className="text-xs text-brand font-mono">Earned: à§³ 25,000 â€¢ Received 5-Star Feedback</p>
          </div>
          <div className="glass-panel p-6 rounded-xl border-l-4 border-cyberBlue hover:-translate-y-1 transition-transform">
            <div className="flex justify-between items-start mb-2">
              <h4 className="font-bold text-white text-lg">Hackathon Winner</h4>
              <span className="text-xs bg-cyberBlue/20 text-cyberBlue px-2 py-1 rounded font-bold">NSU Cybernauts</span>
            </div>
            <p className="text-gray-400 text-sm mb-4">Secured 1st Runner Up in the 2025 Web Dev Sprint.</p>
            <p className="text-xs text-cyberBlue font-mono">Role: Lead React Developer</p>
          </div>
        </div>
      </motion.div>
    );

    // 3. The SafePay Wallet View
    const WalletView = () => (
      <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} className="p-8 max-w-4xl mx-auto space-y-8">
        <div className="bg-gradient-to-br from-brand/20 to-obsidian p-8 rounded-2xl border border-brand/30 relative overflow-hidden">
          <div className="absolute right-0 top-0 w-64 h-full bg-brand opacity-10 filter blur-3xl"></div>
          <div className="relative z-10 flex justify-between items-center">
            <div>
              <p className="text-sm font-bold text-gray-400 uppercase tracking-widest mb-2">Total Escrow Balance</p>
              <h2 className="text-5xl font-black text-white tracking-tight">à§³ 14,500<span className="text-lg text-gray-500 font-normal">.00</span></h2>
              <p className="text-xs text-success mt-2 flex items-center gap-1">
                <span className="w-2 h-2 rounded-full bg-success animate-pulse"></span> bKash / Nagad Pipeline Active
              </p>
            </div>
            <button className="bg-white text-black px-8 py-4 rounded-xl font-black uppercase tracking-wide hover:bg-gray-200 transition-colors shadow-[0_0_20px_rgba(255,255,255,0.2)]">
              Withdraw Funds
            </button>
          </div>
        </div>

        <h3 className="text-xl font-bold uppercase tracking-widest text-gray-300">Active Escrow Contracts (SafePay)</h3>
        <div className="space-y-4">
          <div className="glass-panel p-5 rounded-xl flex justify-between items-center border-l-4 border-brand">
            <div>
              <h4 className="font-bold text-white">Logo Design - Bean & Byte Cafe</h4>
              <p className="text-xs text-gray-400 mt-1">Client deposited funds. Waiting for your final delivery.</p>
            </div>
            <div className="text-right">
              <p className="text-lg font-black text-brand">à§³ 8,000</p>
              <p className="text-xs text-gray-500 uppercase font-bold">Locked in Vault</p>
            </div>
          </div>
          <div className="glass-panel p-5 rounded-xl flex justify-between items-center border-l-4 border-success">
            <div>
              <h4 className="font-bold text-white">Python Tutoring - CSE115</h4>
              <p className="text-xs text-gray-400 mt-1">Session completed. Client approved.</p>
            </div>
            <div className="text-right">
              <p className="text-lg font-black text-success">à§³ 1,500</p>
              <p className="text-xs text-success uppercase font-bold">Ready to Withdraw</p>
            </div>
          </div>
        </div>
      </motion.div>
    );

    // 4. Interactive Quest Feed (from previous iteration but refined)
    const QuestFeed = () => {
      const [activeQuestId, setActiveQuestId] = useState(null);

      return (
        <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="p-4 md:p-8 space-y-6 max-w-5xl mx-auto">
          {/* Cybernauts Hero Banner */}
          <div className="bg-gradient-to-r from-obsidian to-surface border border-gray-800 rounded-2xl p-8 relative overflow-hidden group">
            <div className="absolute top-0 right-0 w-full h-full bg-[url('https://images.unsplash.com/photo-1550751827-4bd374c3f58b?auto=format&fit=crop&w=1200&q=80')] bg-cover bg-center opacity-20 mix-blend-overlay group-hover:scale-105 transition-transform duration-700"></div>
            <div className="absolute top-0 right-0 w-64 h-64 bg-cyberBlue rounded-full filter blur-[120px] opacity-20"></div>
            <div className="relative z-10">
              <span className="px-3 py-1 bg-cyberBlue/10 text-cyberBlue border border-cyberBlue/30 rounded-full text-xs font-bold uppercase tracking-widest mb-4 inline-block">Official Talent Partner</span>
              <h2 className="text-3xl md:text-5xl font-black text-white mb-4 leading-tight">NSU Cybernauts <br/><span className="neon-text">TechFest 2026</span></h2>
              <p className="text-gray-300 max-w-xl mb-6">Find your dream hackathon squad, apply for event-specific gigs, and showcase your skills to top tech recruiters attending the event.</p>
            </div>
          </div>

          <div className="flex justify-between items-center border-b border-gray-800 pb-4">
            <h3 className="text-xl font-bold uppercase tracking-widest text-gray-300">Live Mission Board</h3>
          </div>

          {questsData.map(quest => (
            <motion.div layout key={quest.id} onClick={() => setActiveQuestId(activeQuestId === quest.id ? null : quest.id)}
              className={`border rounded-xl p-6 cursor-pointer transition-all duration-300 ${activeQuestId === quest.id ? 'bg-surface border-brand shadow-[0_0_30px_rgba(255,115,0,0.15)]' : 'bg-card border-gray-800 hover:border-gray-600'}`}>
              <div className="flex justify-between items-start">
                <div className="flex gap-4">
                  <img src={quest.employerImg} className="w-12 h-12 rounded-lg object-cover border border-gray-700" />
                  <div>
                    <p className="text-xs text-gray-400 font-bold uppercase tracking-wider mb-1">{quest.employer}</p>
                    <h3 className="text-xl font-bold text-white">{quest.title}</h3>
                  </div>
                </div>
              </div>

              <div className="flex gap-2 mt-4 flex-wrap items-center">
                <span className="px-3 py-1 bg-gray-800 border border-gray-700 rounded-md text-xs font-semibold text-white uppercase">{quest.category}</span>
                <span className="px-3 py-1 bg-success/20 border border-success/30 rounded-md text-xs font-semibold text-success uppercase">{quest.pay}</span>
                <span className="ml-auto text-brand text-sm font-black">{quest.xpReward}</span>
              </div>

              <AnimatePresence>
                {activeQuestId === quest.id && (
                  <motion.div initial={{ height: 0, opacity: 0 }} animate={{ height: "auto", opacity: 1 }} exit={{ height: 0, opacity: 0 }} className="overflow-hidden">
                    <div className="pt-6 mt-6 border-t border-gray-800">
                      <p className="text-gray-300 text-sm leading-relaxed mb-6 bg-obsidian p-4 rounded-lg border border-gray-800">{quest.fullDesc}</p>
                      <button className="w-full py-4 bg-brand hover:bg-brandHover text-white font-black rounded-xl shadow-[0_0_20px_rgba(255,115,0,0.3)] transition-transform transform hover:-translate-y-1 uppercase tracking-widest"
                      onClick={(e) => { e.stopPropagation(); alert(`Initiating Smart Contract via SideQuest Protocol...`); }}>
                        {quest.linkText}
                      </button>
                    </div>
                  </motion.div>
                )}
              </AnimatePresence>
            </motion.div>
          ))}
        </motion.div>
      );
    };

    // 5. Main App Assembly
    const App = () => {
      const [loading, setLoading] = useState(true);
      const [activeTab, setActiveTab] = useState('quests');

      if (loading) return <BootScreen onComplete={() => setLoading(false)} />;

      return (
        <div className="h-screen flex flex-col font-sans">
          {/* Header */}
          <header className="glass-panel px-6 md:px-12 py-4 flex justify-between items-center z-50 border-b border-gray-800">
            <h1 className="text-2xl font-black tracking-tighter cursor-pointer" onClick={() => setActiveTab('quests')}>Side<span className="text-brand">Quest</span></h1>
            
            <nav className="hidden md:flex bg-obsidian border border-gray-800 rounded-full p-1">
              {['quests', 'portfolio', 'wallet'].map(tab => (
                <button key={tab} onClick={() => setActiveTab(tab)}
                  className={`px-6 py-2 rounded-full text-xs font-bold uppercase tracking-widest transition-all ${activeTab === tab ? 'bg-brand text-white shadow-[0_0_15px_rgba(255,115,0,0.4)]' : 'text-gray-400 hover:text-white'}`}>
                  {tab}
                </button>
              ))}
            </nav>

            <div className="flex items-center gap-3">
              <div className="text-right hidden sm:block">
                <p className="text-sm font-bold text-white">Student Demo</p>
                <p className="text-xs text-brand font-mono">LVL 12 â€¢ NSU</p>
              </div>
<img
              src="https://i.ibb.co/cKwJ2pWB/FB-IMG-1781330470425.jpg"
              onClick={() => setActiveTab('portfolio')}
              className="w-10 h-10 rounded-full border-2 border-brand cursor-pointer"
            />
            </div>
          </header>

          {/* Main Content Area Routing */}
          <main className="flex-1 overflow-y-auto scroll-smooth pb-20">
            <AnimatePresence mode="wait">
              {activeTab === 'quests' && <motion.div key="quests" initial={{opacity:0}} animate={{opacity:1}} exit={{opacity:0}}><QuestFeed /></motion.div>}
              {activeTab === 'portfolio' && <motion.div key="portfolio" initial={{opacity:0}} animate={{opacity:1}} exit={{opacity:0}}><PortfolioView /></motion.div>}
              {activeTab === 'wallet' && <motion.div key="wallet" initial={{opacity:0}} animate={{opacity:1}} exit={{opacity:0}}><WalletView /></motion.div>}
            </AnimatePresence>
          </main>
        </div>
      );
    };

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
  </script>
</body>
</html>
