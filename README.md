import React, { useState, useEffect } from 'react';
import { 
  Heart, Users, TrendingUp, BookOpen, ShieldAlert, 
  Send, Smile, Star, CheckCircle, AlertCircle, ChevronRight, 
  X, User, Info, Award, Calendar, Zap, Coffee, Sparkles, Brain, 
  Bell, Layout, Search, ArrowRight, ShieldCheck, Lock, BookMarked,
  Lightbulb, Shield, Flame, Compass, Phone, Mic, MessageCircle,
  Share2, Ghost, Volume2, Headphones, Bot
} from 'lucide-react';

// --- Mock Data ---
const ARTICLES = [
  { id: 'a1', title: "Ansiedade Pré-Prova?", desc: "Técnicas para focar no presente.", content: "A ansiedade é natural. Tente a técnica 5-4-3-2-1...", category: "Estudos", readTime: "3 min", icon: Brain, color: 'bg-blue-50 text-blue-600' },
  { id: 'a2', title: "Amizades Verdadeiras", desc: "Como identificar conexões saudáveis.", content: "Amizade deve trazer paz, não cansaço...", category: "Social", readTime: "5 min", icon: Users, color: 'bg-pink-50 text-pink-600' },
  { id: 'a3', title: "O Poder do Sono", desc: "Durma bem para render mais.", content: "O sono limpa as toxinas do cérebro...", category: "Saúde", readTime: "4 min", icon: Zap, color: 'bg-yellow-50 text-yellow-600' }
];

const SUPPORT_GROUPS = [
  { id: 'g1', name: "Foco nos Estudos", icon: BookOpen, members: "1.2k", color: "from-blue-500 to-cyan-500", tag: "Acadêmico", chat: [{ id: 1, user: "Mediador Silva", text: "Olá! Como estão as revisões?", role: "mod" }] },
  { id: 'g2', name: "Escudo Ativo", icon: ShieldAlert, members: "850", color: "from-orange-500 to-red-500", tag: "Bullying", chat: [{ id: 1, user: "Mediador Ana", text: "Este é um espaço seguro focado em apoio mútuo.", role: "mod" }] },
  { id: 'g3', name: "Expressão Criativa", icon: Sparkles, members: "2.1k", color: "from-purple-500 to-pink-500", tag: "Arte", chat: [] },
  { id: 'g4', name: "Primeira Vez Aqui", icon: Compass, members: "3.4k", color: "from-green-500 to-teal-500", tag: "Acolhimento", chat: [] },
  { id: 'g5', name: "Superando Perdas", icon: Heart, members: "1.1k", color: "from-indigo-500 to-blue-500", tag: "Luto", chat: [] }
];

const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [activeTab, setActiveTab] = useState('home');
  const [mood, setMood] = useState(null);
  const [userName, setUserName] = useState("");
  const [feedPosts, setFeedPosts] = useState([
    { id: 1, user: "Viajante Estelar", text: "Hoje consegui terminar minha tarefa acumulada. Um passo de cada vez! ✨", hugs: 15, supports: 8, time: "Há 5min" },
    { id: 2, user: "Alma Corajosa", text: "Alguém mais sente que o domingo à noite é meio melancólico?", hugs: 32, supports: 12, time: "Há 1h" }
  ]);
  const [newPostText, setNewPostText] = useState("");
  const [hearts, setHearts] = useState([]);
  const [selectedGroup, setSelectedGroup] = useState(null);
  const [isInGroupChat, setIsInGroupChat] = useState(false);
  const [readingArticle, setReadingArticle] = useState(null);
  const [isZenCalling, setIsZenCalling] = useState(false);

  // Efeito Visual de Corações
  const triggerHearts = () => {
    const id = Date.now();
    setHearts(prev => [...prev, id]);
    setTimeout(() => setHearts(prev => prev.filter(h => h !== id)), 2000);
  };

  const handleCreatePost = (e) => {
    e.preventDefault();
    if (!newPostText.trim()) return;
    const anonName = "Explorador Anon";
    setFeedPosts([{ id: Date.now(), user: anonName, text: newPostText, hugs: 0, supports: 0, time: "Agora" }, ...feedPosts]);
    setNewPostText("");
    triggerHearts();
  };

  const NavItem = ({ id, icon: Icon, label }) => (
    <button 
      onClick={() => {setActiveTab(id); setReadingArticle(null); setIsInGroupChat(false)}}
      className={`flex flex-col items-center gap-1 p-2 transition-all ${activeTab === id ? 'text-purple-600 scale-110' : 'text-gray-400 hover:text-purple-400'}`}
    >
      <Icon size={24} />
      <span className="text-[10px] font-bold uppercase tracking-widest">{label}</span>
    </button>
  );

  // --- Telas ---

  if (!isLoggedIn) return (
    <div className="min-h-screen bg-[#0F0A1E] flex items-center justify-center p-6 font-sans">
      <div className="w-full max-w-[440px] bg-white/5 backdrop-blur-xl border border-white/10 rounded-[3rem] p-10 text-center shadow-2xl">
        <div className="inline-flex p-4 rounded-3xl bg-purple-600 mb-6 shadow-lg shadow-purple-500/30">
          <Heart className="text-white fill-white" size={40} />
        </div>
        <h1 className="text-4xl font-black text-white mb-2 tracking-tighter italic">REFÚGIO</h1>
        <p className="text-purple-300/60 text-sm mb-10 uppercase tracking-widest font-bold">Espaço Seguro & Anônimo</p>
        
        <div className="space-y-4">
          <input 
            type="text" 
            value={userName}
            onChange={(e) => setUserName(e.target.value)}
            placeholder="Escolha um pseudônimo..." 
            className="w-full p-5 bg-white/5 border border-white/10 rounded-2xl text-white placeholder:text-white/20 outline-none focus:ring-2 focus:ring-purple-500 transition-all"
          />
          <button 
            onClick={() => userName && setIsLoggedIn(true)}
            className="w-full bg-purple-600 hover:bg-purple-500 text-white font-black py-5 rounded-2xl transition-all shadow-xl shadow-purple-900/20 active:scale-95"
          >
            ENTRAR NO REFÚGIO
          </button>
        </div>
        <p className="mt-8 text-[11px] text-white/40 uppercase font-bold leading-relaxed tracking-wider">
          Sua privacidade é nossa prioridade. Nada do que você disser será vinculado à sua identidade real.
        </p>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-[#FDFDFF] text-slate-900 font-sans pb-32">
      {/* Sistema de Animação de Corações */}
      <div className="fixed inset-0 pointer-events-none z-50">
        {hearts.map(id => (
          <div key={id} className="absolute bottom-20 left-1/2 -translate-x-1/2">
            {[...Array(6)].map((_, i) => (
              <div key={i} className="absolute animate-bounce opacity-0" 
                style={{ 
                  left: `${Math.random() * 200 - 100}px`, 
                  animation: `float-up 2s ease-out forwards`,
                  animationDelay: `${Math.random() * 0.5}s`
                }}>
                <Heart fill="#9333ea" className="text-purple-600" size={30} />
              </div>
            ))}
          </div>
        ))}
      </div>

      <style>{`
        @keyframes float-up {
          0% { transform: translateY(0) scale(0.5); opacity: 0; }
          20% { opacity: 1; }
          100% { transform: translateY(-500px) scale(1.5); opacity: 0; }
        }
      `}</style>

      {/* Header */}
      <header className="sticky top-0 bg-white/80 backdrop-blur-xl border-b p-4 px-6 flex justify-between items-center z-40">
        <div className="flex items-center gap-2 font-black text-2xl tracking-tighter cursor-pointer" onClick={() => setActiveTab('home')}>
          <Heart className="text-purple-600" fill="currentColor" /> REFÚGIO
        </div>
        <div className="flex items-center gap-4">
          <div className="text-[10px] font-black bg-purple-50 text-purple-600 px-3 py-1 rounded-full uppercase tracking-tighter">
            {userName}
          </div>
          <button className="p-2 bg-slate-50 rounded-full text-slate-400"><Bell size={20}/></button>
        </div>
      </header>

      <main className="max-w-6xl mx-auto p-6">
        {activeTab === 'home' && !readingArticle && (
          <div className="space-y-8 animate-in fade-in slide-in-from-bottom-4 duration-700">
            {/* Boas-vindas & Mood */}
            <section className="bg-white border-2 border-slate-100 rounded-[2.5rem] p-8 shadow-sm flex flex-col md:flex-row justify-between items-center gap-6">
              <div>
                <h2 className="text-3xl font-black tracking-tight flex items-center gap-3">
                  Como está seu coração hoje, <span className="text-purple-600">{userName}</span>?
                </h2>
                <p className="text-slate-400 font-medium text-sm">Apenas você pode ver seu nome aqui. O resto do mundo te vê como anônimo.</p>
              </div>
              <div className="flex gap-3">
                {['😊', '😐', '😔', '😫', '😡'].map(e => (
                  <button 
                    key={e} 
                    onClick={() => {setMood(e); triggerHearts()}}
                    className={`text-2xl w-14 h-14 flex items-center justify-center rounded-2xl transition-all ${mood === e ? 'bg-purple-600 shadow-lg scale-110 shadow-purple-200 text-white' : 'bg-slate-50 hover:bg-slate-100'}`}
                  >
                    {e}
                  </button>
                ))}
              </div>
            </section>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
              {/* Feed Central */}
              <div className="lg:col-span-2 space-y-8">
                <div className="bg-white border-2 border-slate-100 rounded-[2.5rem] p-8 shadow-sm">
                  <h3 className="text-xl font-black mb-6 flex items-center gap-2"><TrendingUp size={24} className="text-purple-600"/> Mural de Pensamentos</h3>
                  <form onSubmit={handleCreatePost} className="mb-8">
                    <textarea 
                      value={newPostText}
                      onChange={(e) => setNewPostText(e.target.value)}
                      placeholder="O que está na sua mente? Ninguém saberá que é você..."
                      className="w-full bg-slate-50 p-6 rounded-[1.5rem] mb-4 outline-none border-2 border-transparent focus:border-purple-100 resize-none h-32 font-medium"
                    />
                    <div className="flex justify-between items-center">
                      <p className="text-[10px] font-bold text-slate-400 uppercase tracking-widest flex items-center gap-1">
                        <Shield size={12}/> Espaço 100% Protegido
                      </p>
                      <button className="bg-purple-600 hover:bg-purple-700 text-white px-8 py-3 rounded-xl font-black flex items-center gap-2 transition-all active:scale-95">
                        SOLTAR NO MUNDO <Send size={18}/>
                      </button>
                    </div>
                  </form>

                  <div className="space-y-6">
                    {feedPosts.map(p => (
                      <div key={p.id} className="p-6 bg-slate-50/50 rounded-[2rem] border border-slate-100 hover:border-purple-100 transition-all">
                        <div className="flex justify-between items-start mb-4">
                          <div className="flex items-center gap-3">
                            <div className="w-10 h-10 bg-white rounded-full flex items-center justify-center border border-slate-100 shadow-sm text-purple-600">
                              <Ghost size={20}/>
                            </div>
                            <span className="text-xs font-black uppercase tracking-tighter text-slate-400">{p.user}</span>
                          </div>
                          <span className="text-[10px] font-bold text-slate-300 uppercase">{p.time}</span>
                        </div>
                        <p className="text-slate-700 leading-relaxed font-medium text-lg mb-6">{p.text}</p>
                        <div className="flex gap-6">
                          <button 
                            onClick={() => {triggerHearts()}}
                            className="flex items-center gap-2 text-xs font-black text-purple-600 hover:bg-purple-50 px-4 py-2 rounded-full transition-all"
                          >
                            <Heart size={16} /> {p.hugs} Abraços
                          </button>
                          <button className="flex items-center gap-2 text-xs font-black text-slate-400 hover:bg-slate-100 px-4 py-2 rounded-full transition-all">
                            <ShieldCheck size={16} /> Apoiar
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>

              {/* Sidebar Direita */}
              <div className="space-y-8">
                <div 
                  onClick={() => setIsZenCalling(true)}
                  className="bg-gradient-to-br from-indigo-600 to-purple-700 p-8 rounded-[2.5rem] text-white cursor-pointer hover:scale-[1.02] active:scale-95 transition-all shadow-xl shadow-purple-200 group"
                >
                  <div className="bg-white/20 w-16 h-16 rounded-2xl flex items-center justify-center mb-6 group-hover:rotate-12 transition-all">
                    <Headphones size={32} />
                  </div>
                  <h4 className="text-2xl font-black mb-1">Momento Zen</h4>
                  <p className="text-xs opacity-70 mb-8 uppercase font-bold tracking-widest">Meditação Guiada via Áudio</p>
                  <div className="bg-white text-purple-700 py-4 rounded-2xl font-black text-center text-sm">
                    ATENDER CHAMADA
                  </div>
                </div>

                <div className="bg-white border-2 border-slate-100 p-8 rounded-[2.5rem] shadow-sm">
                  <h4 className="font-black text-lg mb-6 flex items-center gap-2"><BookMarked size={20} className="text-purple-600"/> Biblioteca da Mente</h4>
                  <div className="space-y-4">
                    {ARTICLES.map(art => (
                      <div 
                        key={art.id} 
                        onClick={() => setReadingArticle(art)}
                        className="p-4 bg-slate-50 rounded-2xl cursor-pointer hover:bg-purple-50 transition-all border-2 border-transparent hover:border-purple-100 flex items-center gap-4"
                      >
                        <div className={`w-12 h-12 rounded-xl flex items-center justify-center ${art.color}`}>
                          <art.icon size={20} />
                        </div>
                        <div>
                          <p className="text-sm font-black text-slate-800 leading-tight">{art.title}</p>
                          <span className="text-[10px] text-slate-400 uppercase font-bold">{art.readTime} leitura</span>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* Tab Grupos de Apoio */}
        {activeTab === 'groups' && !isInGroupChat && (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 animate-in slide-in-from-bottom-8">
            {SUPPORT_GROUPS.map(g => (
              <div 
                key={g.id} 
                onClick={() => {setSelectedGroup(g); setIsInGroupChat(true)}}
                className="bg-white border-2 border-slate-100 p-8 rounded-[3rem] cursor-pointer hover:shadow-xl hover:-translate-y-2 transition-all relative overflow-hidden group"
              >
                <div className={`absolute top-0 right-0 w-32 h-32 bg-gradient-to-br ${g.color} opacity-5 -mr-16 -mt-16 rounded-full group-hover:scale-150 transition-all`} />
                <div className={`w-16 h-16 rounded-[1.5rem] bg-gradient-to-br ${g.color} mb-6 flex items-center justify-center text-white shadow-lg`}>
                  <g.icon size={32} />
                </div>
                <h3 className="text-2xl font-black mb-2 tracking-tight">{g.name}</h3>
                <div className="flex items-center gap-2 mb-6">
                  <span className="text-[10px] bg-slate-100 px-3 py-1 rounded-full font-black text-slate-500 uppercase">{g.tag}</span>
                  <span className="text-[10px] font-bold text-slate-400">{g.members} MEMBROS</span>
                </div>
                <p className="text-slate-500 text-sm font-medium leading-relaxed mb-6">Um lugar seguro para conversar com mediação profissional.</p>
                <div className="flex items-center gap-2 text-purple-600 font-black text-xs uppercase tracking-widest">
                  Entrar no Grupo <ArrowRight size={14}/>
                </div>
              </div>
            ))}
          </div>
        )}

        {/* Tab Aurora (IA) */}
        {activeTab === 'aurora' && (
          <div className="max-w-4xl mx-auto h-[70vh] flex flex-col animate-in fade-in zoom-in-95">
            <div className="bg-white border-2 border-slate-100 rounded-[3rem] flex-1 flex flex-col overflow-hidden shadow-2xl">
              <div className="p-8 border-b-2 border-slate-50 flex items-center gap-4">
                <div className="w-14 h-14 bg-purple-600 rounded-2xl flex items-center justify-center text-white shadow-lg shadow-purple-100">
                  <Bot size={32} />
                </div>
                <div>
                  <h4 className="font-black text-xl">Aurora</h4>
                  <p className="text-[10px] text-slate-400 uppercase font-black tracking-widest">Sua assistente de escuta ativa</p>
                </div>
              </div>
              <div className="flex-1 p-8 overflow-y-auto space-y-6 bg-slate-50/30">
                 <div className="flex justify-start">
                    <div className="bg-white border-2 border-purple-50 p-6 rounded-[2rem] rounded-bl-none max-w-[80%] shadow-sm">
                      <p className="text-slate-700 font-medium leading-relaxed">Olá, <span className="font-bold text-purple-600">{userName}</span>. Eu sou a Aurora. Estou aqui para te ouvir de forma anônima e gentil. Como você se sente agora?</p>
                    </div>
                 </div>
              </div>
              <div className="p-6 bg-white border-t-2 border-slate-50 flex gap-4">
                <input type="text" placeholder="Fale com a Aurora..." className="flex-1 bg-slate-100 p-5 rounded-2xl outline-none focus:ring-2 focus:ring-purple-500 transition-all font-medium" />
                <button className="bg-purple-600 text-white w-16 h-16 rounded-2xl flex items-center justify-center shadow-lg active:scale-95 transition-all"><Mic size={24}/></button>
              </div>
            </div>
          </div>
        )}

        {/* Chat de Grupo Ativo */}
        {isInGroupChat && selectedGroup && (
          <div className="max-w-4xl mx-auto bg-white rounded-[3rem] border-2 border-slate-100 h-[70vh] flex flex-col overflow-hidden shadow-2xl animate-in zoom-in-95">
            <div className={`p-8 bg-gradient-to-r ${selectedGroup.color} text-white flex justify-between items-center`}>
              <div className="flex items-center gap-4">
                <div className="w-12 h-12 bg-white/20 rounded-2xl flex items-center justify-center backdrop-blur-md">
                  <selectedGroup.icon size={24} />
                </div>
                <div>
                  <h4 className="font-black text-xl tracking-tight">{selectedGroup.name}</h4>
                  <p className="text-[10px] opacity-80 uppercase font-bold tracking-widest">Sala de Apoio Ativa</p>
                </div>
              </div>
              <button onClick={() => setIsInGroupChat(false)} className="p-3 bg-white/10 rounded-full hover:bg-white/20 transition-all">
                <X size={24} />
              </button>
            </div>
            <div className="flex-1 p-8 overflow-y-auto space-y-6 bg-slate-50/50">
              {selectedGroup.chat.map(m => (
                <div key={m.id} className={`flex ${m.role === 'mod' ? 'justify-start' : 'justify-end'}`}>
                  <div className={`max-w-[80%] p-6 rounded-[2rem] text-sm font-medium ${m.role === 'mod' ? 'bg-white border-2 border-purple-100 text-slate-700 rounded-bl-none' : 'bg-purple-600 text-white rounded-br-none shadow-lg'}`}>
                    <div className="flex items-center gap-2 mb-2">
                      <span className={`text-[10px] font-black uppercase ${m.role === 'mod' ? 'text-purple-600' : 'text-purple-200'}`}>{m.user}</span>
                      {m.role === 'mod' && <ShieldCheck size={12} className="text-purple-600"/>}
                    </div>
                    {m.text}
                  </div>
                </div>
              ))}
            </div>
            <div className="p-6 bg-white border-t-2 border-slate-50 flex gap-4">
              <input type="text" placeholder="Escreva algo gentil..." className="flex-1 bg-slate-100 p-5 rounded-2xl outline-none focus:ring-2 focus:ring-purple-500 transition-all font-medium" />
              <button className="bg-purple-600 text-white w-16 h-16 rounded-2xl flex items-center justify-center shadow-lg active:scale-95 transition-all"><Send size={24}/></button>
            </div>
          </div>
        )}

        {/* Tab Emergência / Ajuda */}
        {activeTab === 'emergency' && (
          <div className="max-w-2xl mx-auto space-y-8 text-center py-12 animate-in zoom-in-95">
             <div className="inline-flex p-8 rounded-full bg-red-50 text-red-500 mb-4 shadow-inner">
               <ShieldAlert size={64} strokeWidth={2.5} />
             </div>
             <h2 className="text-4xl font-black tracking-tight">Você não está sozinho.</h2>
             <p className="text-slate-500 text-lg font-medium">Se você está sentindo que as coisas estão difíceis demais agora, existem profissionais e serviços prontos para te acolher.</p>
             
             <div className="grid gap-4 mt-12 text-left">
                {/* CVV */}
                <a href="tel:188" className="p-6 bg-white border-2 border-red-100 rounded-[2.5rem] flex items-center justify-between hover:bg-red-50 transition-all group">
                  <div className="flex items-center gap-5">
                    <div className="w-14 h-14 bg-red-500 rounded-2xl flex items-center justify-center text-white shadow-lg">
                      <Phone size={24} />
                    </div>
                    <div>
                      <p className="font-black text-xl text-slate-800">CVV</p>
                      <p className="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Apoio Emocional - 24h</p>
                    </div>
                  </div>
                  <div className="bg-red-500 text-white px-5 py-3 rounded-xl font-black text-sm">188</div>
                </a>

                {/* SAMU */}
                <a href="tel:192" className="p-6 bg-white border-2 border-slate-100 rounded-[2.5rem] flex items-center justify-between hover:bg-slate-50 transition-all group">
                  <div className="flex items-center gap-5">
                    <div className="w-14 h-14 bg-orange-500 rounded-2xl flex items-center justify-center text-white shadow-lg">
                      <Phone size={24} />
                    </div>
                    <div>
                      <p className="font-black text-xl text-slate-800">SAMU</p>
                      <p className="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Emergência Médica</p>
                    </div>
                  </div>
                  <div className="bg-orange-500 text-white px-5 py-3 rounded-xl font-black text-sm">192</div>
                </a>

                {/* POLICIA */}
                <a href="tel:190" className="p-6 bg-white border-2 border-slate-100 rounded-[2.5rem] flex items-center justify-between hover:bg-slate-50 transition-all group">
                  <div className="flex items-center gap-5">
                    <div className="w-14 h-14 bg-blue-600 rounded-2xl flex items-center justify-center text-white shadow-lg">
                      <Shield size={24} />
                    </div>
                    <div>
                      <p className="font-black text-xl text-slate-800">Polícia Militar</p>
                      <p className="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Segurança e Emergência</p>
                    </div>
                  </div>
                  <div className="bg-blue-600 text-white px-5 py-3 rounded-xl font-black text-sm">190</div>
                </a>
             </div>
          </div>
        )}
      </main>

      {/* Navegação Inferior */}
      <nav className="fixed bottom-8 left-1/2 -translate-x-1/2 bg-white/90 backdrop-blur-xl border-2 border-slate-100 rounded-[3rem] px-8 py-4 flex gap-8 shadow-2xl z-50">
        <NavItem id="home" icon={Layout} label="Início" />
        <NavItem id="groups" icon={Users} label="Grupos" />
        <NavItem id="aurora" icon={Bot} label="Aurora" />
        <NavItem id="emergency" icon={ShieldAlert} label="Ajuda" />
      </nav>

      {/* Overlay de Chamada Zen */}
      {isZenCalling && (
        <div className="fixed inset-0 bg-indigo-950 z-[100] flex flex-col items-center justify-center text-white p-10 text-center animate-in fade-in duration-1000">
          <div className="w-40 h-40 bg-white/10 rounded-full flex items-center justify-center mb-12 animate-pulse relative">
            <div className="absolute inset-0 rounded-full border-4 border-white/20 animate-ping" />
            <User size={80} className="text-white/60" />
          </div>
          <h2 className="text-4xl font-black mb-4 tracking-tight">Chamada Zen Ativa</h2>
          <p className="text-indigo-200 uppercase text-xs tracking-[0.3em] font-black mb-16">Respire fundo junto com a música...</p>
          <button 
            onClick={() => setIsZenCalling(false)}
            className="w-24 h-24 bg-red-500 rounded-full flex items-center justify-center rotate-[135deg] shadow-2xl active:scale-90 transition-all"
          >
            <Phone size={40} fill="white"/>
          </button>
        </div>
      )}
    </div>
  );
};

export default App;
