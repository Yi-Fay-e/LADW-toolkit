import React, { useState, useEffect, useMemo } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getFirestore, 
  collection, 
  doc, 
  setDoc, 
  onSnapshot, 
  query, 
  deleteDoc, 
  updateDoc,
  enableIndexedDbPersistence,
  writeBatch
} from 'firebase/firestore';
import { 
  getAuth, 
  signInWithCustomToken, 
  signInAnonymously, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  Sparkles, 
  History, 
  Star, 
  Info,
  Clock,
  ArrowLeft,
  ChevronLeft,
  Cloud, 
  CloudOff,
  Zap,
  Target,
  User,
  Users,
  Tag,
  ChevronRight,
  Sun,
  Moon,
  Filter,
  ChevronDown,
  Edit2,
  Trash2,
  Download,
  Upload,
  Crown,
  BarChart3
} from 'lucide-react';

// --- Firebase 配置 ---
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = 'love-and-deepspace-yifaye';

try {
  enableIndexedDbPersistence(db).catch((err) => {
    if (err.code === 'failed-precondition') {
      console.warn('Firestore Persistence failed: Multiple tabs open');
    } else if (err.code === 'unimplemented') {
      console.warn('Firestore Persistence failed: Browser not supported');
    }
  });
} catch (e) {
  console.error("Persistence error", e);
}

// --- 專屬圖標組件 ---
function CreatorCat({ size = 14, className = "" }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
      <path d="M12 5c.67 0 1.35.09 2 .26 1.78-2 5.03-2.84 6.42-2.26 1.4.58-.42 7-.42 7 .57 1.27.75 2.53.57 3.83-.25 1.74-1.05 3.06-2.14 4.03C17.15 19.04 15 20 12 20s-5.15-.96-6.43-2.14c-1.09-.97-1.89-2.29-2.14-4.03-.18-1.3 0-2.56.57-3.83 0 0-1.82-6.42-.42-7 1.39-.58 4.64.26 6.42 2.26.65-.17 1.33-.26 2-.26Z" />
      <path d="M7 14H2" />
      <path d="M7 15.5l-4.5 1.5" />
      <path d="M17 14h5" />
      <path d="M17 15.5l4.5 1.5" />
      <circle cx="9" cy="12" r="0.8" fill="currentColor" />
      <circle cx="15" cy="12" r="0.8" fill="currentColor" />
    </svg>
  );
}

function ZayneSnowflake({ size = 14, className = "" }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
        <line x1="2" y1="12" x2="22" y2="12" /><line x1="12" y1="2" x2="12" y2="22" /><line x1="4.93" y1="4.93" x2="19.07" y2="19.07" /><line x1="19.07" y1="4.93" x2="4.93" y2="19.07" />
    </svg>
  );
}

function RafayelFish({ size = 14, className = "" }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
      <path d="M18 8a2 2 0 1 1 0 4 2 2 0 1 1 0-4Z" /><path d="M2 12c0 3.4 2.6 6 6 6 1.2 0 2.5-.5 3.5-1.5L22 7" /><path d="M2 12c0-3.4 2.6-6 6-6 1.2 0 2.5.5 3.5 1.5L22 17" /><path d="M10 12h.01" />
    </svg>
  );
}

function SylusFeather({ size = 14, className = "" }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
      <path d="M20.24 12.24a6 6 0 0 0-8.49-8.49L5 10.5V19h8.5l6.74-6.76z" /><line x1="16" y1="8" x2="2" y2="22" /><line x1="17.5" y1="15" x2="9" y2="15" />
    </svg>
  );
}

function CalebApple({ size = 16, className = "" }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
      <path d="M12 21c-1.5 0-3 .5-4.5 0-3.5-1-4.5-4.5-4.5-8s3-6 6.5-6c1 0 1.7.5 2.5.5s1.5-.5 2.5-.5c3.5 0 6.5 2 6.5 6s-1 7-4.5 8c-1.5.5-3 0-4.5 0z" /><path d="M12 7V3" />
    </svg>
  );
}

const CHARACTERS = [
  { 
    id: 'xavier', 
    name: '沈星回', 
    engName: 'Xavier', 
    glowColor: 'rgba(255, 235, 59, 0.7)', 
    dimGlow: 'rgba(255, 235, 59, 0.4)', 
    icon: <Star size={14} fill="currentColor" />, 
    solidColor: 'from-yellow-200 via-yellow-500 to-yellow-600',
    quotes: [
      "如果這個世界真的已經無處可逃，那至少你還可以逃來我身邊",
      "如果下一個春天還很遙遠，我們現在就見面吧",
      "我不會離開，因為你還在這裡",
      "我的光芒，只朝向你在的方向",
      "希望我能成為你的歸屬，永遠在你眼中",
      "還是讓星星自己落下來，留在你身邊吧",
      "可我本來就偏心你",
      "無論多少次，無論你在哪，我都會找到你",
      "樓下的貓很想你，我也是",
      "你在哪裡，光就在哪裡"
    ]
  },
  { 
    id: 'zayne', 
    name: '黎深', 
    engName: 'Zayne', 
    glowColor: 'rgba(56, 189, 248, 0.8)', 
    dimGlow: 'rgba(56, 189, 248, 0.4)', 
    icon: <ZayneSnowflake size={14} />, 
    solidColor: 'from-sky-100 via-sky-500 to-blue-800',
    quotes: [
      "如果是週末見你，我一般從週四開始高興",
      "只要你需要，永遠都會有一場不會消融的雪只為你而下",
      "出格的事，總是讓人上癮",
      "希望你不要生病，不要受傷，不要總是和黎醫生見面，而是多和\"黎深\"見面",
      "你獨佔春天，我獨佔你",
      "靈魂先於我的記憶，認出了你",
      "想你一個人有沒有好好吃飯、照顧好自己的身體，萬一等不到我，會不會生氣",
      "以後也一起看月亮吧，意中人",
      "我最想珍藏、最喜歡的瞬間，一定都有你的身影",
      "我還想要下一個「十年」"
    ]
  }, 
  { 
    id: 'rafayel', 
    name: '祁煜', 
    engName: 'Rafayel', 
    glowColor: 'rgba(219, 112, 147, 0.5)', 
    dimGlow: 'rgba(136, 19, 55, 0.3)', 
    icon: <RafayelFish size={14} />, 
    solidColor: 'from-[#db7da1] via-[#6b21a8] to-black',
    quotes: [
      "等你等了八百年，水母都能走路了，海龜都會爬樹了，連鯊魚都改吃素了",
      "我數著每天的潮漲潮落，日升月起，終於，要等到和你見面的日子了",
      "願深海的祝福，與你我同在",
      "奇怪，今天特別想你",
      "我想看的世界，在你眼裡",
      "我為你許下的願望會一直飄行在海上，因為我的火焰永不熄滅",
      "你剛不是問，大海裡最珍貴的寶貝是什麼嗎？你笑一笑，就知道了",
      "我祝你希望永不滅",
      "只要你會來，等待就值得",
      "又見面了，保鏢小姐"
    ]
  },
  { 
    id: 'sylus', 
    name: '秦徹', 
    engName: 'Sylus', 
    glowColor: 'rgba(239, 68, 68, 0.7)', 
    dimGlow: 'rgba(239, 68, 68, 0.4)', 
    icon: <SylusFeather size={14} />, 
    solidColor: 'from-red-900 to-black',
    quotes: [
      "我不在乎臨空市怎麼樣，我關心的對象是你",
      "靈魂相契，永不背叛",
      "得讓大家看到，我有心上人了",
      "沒有什麼是比靠自己調整好的狀態更有力量",
      "我是不是可以認為，有一天即便沒有鏈路了，你也會選擇站在我身邊",
      "塔爾城也可以開滿鮮花，只為一人開放",
      "想見你，不需要什麼理由",
      "有沒有跟你說過，我一直很喜歡你志在必得的樣子",
      "花瓣帶著你一起出現在龍的夢裡，那龍會期待著每一個夜晚，風和花瓣的來臨",
      "一旦我們雙手相握，從此便生死與共了"
    ]
  },
  { 
    id: 'caleb', 
    name: '夏以晝', 
    engName: 'Caleb', 
    glowColor: 'rgba(251, 146, 60, 0.7)', 
    dimGlow: 'rgba(251, 146, 60, 0.4)', 
    icon: <CalebApple size={16} />, 
    solidColor: 'from-orange-500 via-red-600 to-red-800',
    quotes: [
      "所有朝你打來的風雨，都不該出現在這個世界上",
      "你願意給的，就是我想要的。你想得到的，就是我願意付出的。",
      "汪",
      "下輩子的小海鳥，別再讓我錯過你了",
      "或許我們天生就屬於彼此，妹妹",
      "你將指引我返航的方向",
      "你有沒有想過，我從來都不是你的哥哥",
      "不需要我？好啊，那你需要什麼？我都可以答應",
      "夏以晝的弱點，與你有關",
      "我不罩著你，難道還要別人來"
    ]
  },
  { 
    id: 'other', 
    name: '墊池', 
    engName: 'Pity', 
    glowColor: 'rgba(148, 163, 184, 0.5)', 
    dimGlow: 'rgba(148, 163, 184, 0.3)', 
    icon: <Info size={14} />, 
    solidColor: 'from-slate-700 to-slate-950', 
    quotes: ["請再努力一下，為了你想見的人、想做的事、想成為的自己"] 
  },
];

const MAIN_TABS = [
  { id: 'event', name: '限定池' },
  { id: 'rerun', name: '復刻池' },
  { id: 'special', name: '特殊池' },
  { id: 'standard', name: '常駐池' },
];

export default function App() {
  const [user, setUser] = useState(null);
  const [view, setView] = useState('landing');
  const [activeTab, setActiveTab] = useState('event');
  const [filterChar, setFilterChar] = useState(null);
  const [activeQuote, setActiveQuote] = useState('');
  const [history, setHistory] = useState([]);
  const [selectedEventFilter, setSelectedEventFilter] = useState('all');
  const [pools, setPools] = useState({
    event: { pity: 0 }, rerun: { pity: 0 }, special: { pity: 0 }, standard: { pity: 0 }
  });
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 20;
  const maxPity = 70;

  useEffect(() => {
    let retryCount = 0;
    const maxRetries = 5;
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (error) {
        if (retryCount < maxRetries) {
          const delay = Math.pow(2, retryCount) * 1000;
          retryCount++;
          setTimeout(initAuth, delay);
        }
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, setUser);
    return () => unsubscribe();
  }, []);

  useEffect(() => {
    if (!user) return;
    const historyCol = collection(db, 'artifacts', appId, 'users', user.uid, 'history');
    const unsubHistory = onSnapshot(query(historyCol), (snapshot) => {
      const data = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
      setHistory(data.sort((a, b) => {
        const dateA = new Date(a.eventDate || a.date);
        const dateB = new Date(b.eventDate || b.date);
        if (dateB - dateA !== 0) return dateB - dateA;
        return (b.date || "").localeCompare(a.date || "");
      }));
    }, () => {});
    const poolsDoc = doc(db, 'artifacts', appId, 'users', user.uid, 'state', 'pools');
    const unsubPools = onSnapshot(poolsDoc, (docSnap) => {
      if (docSnap.exists()) setPools(docSnap.data());
      else { setDoc(poolsDoc, { event: { pity: 0 }, rerun: { pity: 0 }, special: { pity: 0 }, standard: { pity: 0 } }); }
    }, () => {});
    return () => { unsubHistory(); unsubPools(); };
  }, [user]);

  useEffect(() => {
    if (filterChar) {
      const quotes = filterChar.quotes || [];
      if (quotes.length > 0) {
        const randomIdx = Math.floor(Math.random() * quotes.length);
        setActiveQuote(quotes[randomIdx]);
      }
    } else {
      setActiveQuote('');
    }
  }, [filterChar]);

  const pityInfo = useMemo(() => {
    const poolHistory = history.filter(r => r.poolType === activeTab);
    if (poolHistory.length === 0) return { remaining: maxPity - (pools[activeTab]?.pity || 0), accumulated: pools[activeTab]?.pity || 0 };
    const latest = poolHistory[0]; 
    return { remaining: latest.remainingAtRecord, accumulated: maxPity - latest.remainingAtRecord };
  }, [history, activeTab, pools]);

  const currentRemaining = pityInfo.remaining;

  const status = useMemo(() => {
    if (activeTab !== 'event' && activeTab !== 'rerun') return { isGuaranteed: false, isTargetReady: false };
    const poolHistory = history.filter(r => (r.poolType === 'event' || r.poolType === 'rerun') && r.recordType === 'pull');
    if (poolHistory.length === 0) return { isGuaranteed: false, isTargetReady: false };
    const last = poolHistory[0];
    return { isGuaranteed: last.isLost5050, isTargetReady: last.subPoolType === 'multi' && (last.isNotTarget || last.isLost5050) };
  }, [history, activeTab]);

  const eventAnalysis = useMemo(() => {
    if (selectedEventFilter === 'all') return null;
    const eventRecords = history
      .filter(r => r.eventName === selectedEventFilter)
      .sort((a, b) => new Date(a.date).getTime() - new Date(b.date).getTime());
    
    const pulls = eventRecords.filter(r => r.recordType === 'pull');
    const pities = eventRecords.filter(r => r.recordType === 'pity');
    
    let totalPullsInEvent = 0;
    const analysisItems = pulls.map((pull) => {
      totalPullsInEvent += pull.totalPulls;
      const char = CHARACTERS.find(c => c.id === pull.character);
      return {
        id: pull.id,
        charName: char?.name || '未知',
        cost: pull.totalPulls,
        accumulated: totalPullsInEvent
      };
    });

    return {
      totalPulls: totalPullsInEvent,
      pullCount: pulls.length,
      pityCount: pities.reduce((acc, curr) => acc + curr.totalPulls, 0),
      distribution: analysisItems.reverse()
    };
  }, [history, selectedEventFilter]);

  const [isModalOpen, setIsModalOpen] = useState(false);
  const [editingId, setEditingId] = useState(null);
  const [recordType, setRecordType] = useState('pull');
  const [subPoolType, setSubPoolType] = useState('single'); 
  const [pityLabel, setPityLabel] = useState('墊池');
  const [cardType, setCardType] = useState('moon');
  const [selectedChar, setSelectedChar] = useState(null);
  const [eventName, setEventName] = useState('');
  const [eventDate, setEventDate] = useState('');
  const [cardName, setCardName] = useState('');
  const [pullCountInput, setPullCountInput] = useState(''); 
  const [remainingPullsInput, setRemainingPullsInput] = useState(70); 
  const [isLost5050, setIsLost5050] = useState(false); 
  const [isNotTarget, setIsNotTarget] = useState(false); 
  const [inputMode, setInputMode] = useState('cost');

  const openWishModal = () => {
    setEditingId(null); setRecordType('pull'); setSubPoolType('single'); setPityLabel('墊池'); setCardType('moon');
    setPullCountInput(pityInfo.accumulated > 0 ? (pityInfo.accumulated + 1).toString() : '');
    setRemainingPullsInput(currentRemaining); setEventName(selectedEventFilter !== 'all' ? selectedEventFilter : '');
    const today = new Date(); setEventDate(today.toISOString().split('T')[0]); setCardName(''); setIsLost5050(false);
    setIsNotTarget(false); setInputMode('cost'); setIsModalOpen(true);
  };

  const openEdit = (record) => {
    setEditingId(record.id); setRecordType(record.recordType || 'pull'); setSubPoolType(record.subPoolType || 'single');
    setPityLabel(record.pityLabel || '墊池'); setCardType(record.cardType || 'moon'); setSelectedChar(record.character);
    setEventName(record.eventName || '');
    let formattedDate = record.eventDate || '';
    if (formattedDate.includes('/')) formattedDate = formattedDate.split('/').map(v => v.padStart(2, '0')).join('-');
    setEventDate(formattedDate); setCardName(record.cardName || '');
    setPullCountInput(record.totalPulls ? record.totalPulls.toString() : '');
    setRemainingPullsInput(record.remainingAtRecord); setIsLost5050(record.isLost5050 || false);
    setIsNotTarget(record.isNotTarget || false); setInputMode(record.recordType === 'pull' ? 'cost' : 'remain');
    setIsModalOpen(true);
  };

  const resetModal = () => {
    setEditingId(null); setSelectedChar(null); setEventName(''); setEventDate(''); setCardName('');
    setPullCountInput(''); setRemainingPullsInput(70); setIsLost5050(false); setIsNotTarget(false); setIsModalOpen(false);
  };

  const submitRecord = async () => {
    if (!user) return;
    const historyCol = collection(db, 'artifacts', appId, 'users', user.uid, 'history');
    const poolsDoc = doc(db, 'artifacts', appId, 'users', user.uid, 'state', 'pools');
    let savedTotalPulls, savedRemaining, newPoolPity;
    if (recordType === 'pull') {
        savedTotalPulls = pullCountInput === '' ? (pityInfo.accumulated + 1) : parseInt(pullCountInput);
        savedRemaining = 70; newPoolPity = 0;
    } else {
        if (inputMode === 'cost') {
            savedTotalPulls = pullCountInput === '' ? (pityInfo.accumulated + 1) : parseInt(pullCountInput);
            savedRemaining = maxPity - savedTotalPulls;
        } else {
            savedRemaining = remainingPullsInput === '' ? 70 : parseInt(remainingPullsInput);
            savedTotalPulls = maxPity - savedRemaining;
        }
        newPoolPity = savedTotalPulls;
    }
    const now = new Date().toISOString(); const displayDate = eventDate.replace(/-/g, '/');
    const recordData = {
      character: selectedChar || 'other', eventName: eventName || '一般活動', eventDate: displayDate || new Date().toLocaleDateString('zh-TW'),
      cardName: cardName || (recordType === 'pity' ? (pityLabel === '墊池' ? '墊池紀錄' : '手動修正') : '未命名思念'),
      cardType: cardType, subPoolType: subPoolType, pityLabel: recordType === 'pity' ? pityLabel : null,
      totalPulls: savedTotalPulls, remainingAtRecord: savedRemaining, recordType: recordType,
      isLost5050: recordType === 'pull' ? isLost5050 : false, isNotTarget: recordType === 'pull' ? isNotTarget : false,
      poolType: activeTab, date: editingId ? (history.find(r => r.id === editingId)?.date || now) : now, updatedAt: now
    };
    try {
      if (editingId) { await updateDoc(doc(historyCol, editingId), recordData); await updateDoc(poolsDoc, { [`${activeTab}.pity`]: newPoolPity }); }
      else { await setDoc(doc(historyCol), recordData); await updateDoc(poolsDoc, { [`${activeTab}.pity`]: newPoolPity }); }
      resetModal();
    } catch (e) { console.error(e); }
  };

  const deleteRecord = async (e, id) => { e.stopPropagation(); if (!user || !id) return; await deleteDoc(doc(db, 'artifacts', appId, 'users', user.uid, 'history', id)); };

  const handleExport = () => {
    const data = { history, pools, exportedAt: new Date().toISOString(), version: '1.0' };
    const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    const dateStr = new Date().toISOString().split('T')[0];
    a.download = `思念許願碎片_${dateStr}.json`;
    a.click();
    URL.revokeObjectURL(url);
  };

  const handleImport = (e) => {
    const file = e.target.files[0];
    if (!file || !user) return;
    const reader = new FileReader();
    reader.onload = async (event) => {
      try {
        const data = JSON.parse(event.target.result);
        if (!data.history || !data.pools) throw new Error('格式不正確');
        const batch = writeBatch(db);
        const historyCol = collection(db, 'artifacts', appId, 'users', user.uid, 'history');
        const poolsDoc = doc(db, 'artifacts', appId, 'users', user.uid, 'state', 'pools');
        data.history.forEach((record) => {
          const { id, ...cleanRecord } = record;
          const newDoc = doc(historyCol);
          batch.set(newDoc, cleanRecord);
        });
        batch.set(poolsDoc, data.pools);
        await batch.commit();
        alert('匯入成功！');
      } catch (err) {
        console.error(err);
        alert('匯入失敗：請確保文件格式正確');
      }
    };
    reader.readAsText(file);
  };

  const displayHistory = filterChar ? history.filter(r => r.character === filterChar.id) : (selectedEventFilter === 'all' ? history.filter(r => r.poolType === activeTab) : history.filter(r => r.eventName === selectedEventFilter));
  const totalItems = displayHistory.length; const totalPages = Math.ceil(totalItems / itemsPerPage);
  const currentItems = displayHistory.slice((currentPage - 1) * itemsPerPage, currentPage * itemsPerPage);
  const eventOptions = useMemo(() => { const names = history.map(h => h.eventName).filter(n => n && n !== '一般活動'); return ['all', ...new Set(names)]; }, [history]);

  if (view === 'landing') {
    return (
      <div className="min-h-screen bg-[#050508] text-slate-200 font-sans flex justify-center overflow-x-hidden">
        <main className="w-full max-w-md bg-black relative z-10 min-h-screen flex flex-col items-center justify-between py-24 px-8 overflow-hidden">
          <div className="absolute top-0 left-0 w-full h-full pointer-events-none opacity-25">
             <div className="absolute top-[-5%] left-[-10%] w-[70%] h-[40%] bg-indigo-500/10 blur-[120px] rounded-full" />
             <div className="absolute bottom-[-5%] right-[-5%] w-[60%] h-[30%] bg-rose-500/10 blur-[100px] rounded-full" />
          </div>
          <div className="text-center relative z-10 w-full flex flex-col items-center">
            <h1 className="text-5xl font-light tracking-[0.05em] bg-gradient-to-b from-white to-slate-500 bg-clip-text text-transparent italic mb-[-2px]" style={{ fontFamily: 'cursive, serif' }}>Love</h1>
            <p className="text-[10px] text-slate-500 font-bold tracking-[0.4em] uppercase mb-1">AND</p>
            <h2 className="text-3xl font-light tracking-[0.25em] bg-gradient-to-r from-slate-400 via-white to-slate-400 bg-clip-text text-transparent uppercase">Deepspace</h2>
          </div>
          <div className="w-full relative z-10 py-4 grid grid-cols-3 gap-y-10 gap-x-1 px-2 justify-items-center">
            {CHARACTERS.map(c => (
              <button key={c.id} onClick={() => { setFilterChar(c); setView('main'); }} className="group flex flex-col items-center gap-3 active:scale-95 transition-all">
                <div className="w-16 h-16 rounded-full flex items-center justify-center transition-all duration-300 group-hover:brightness-125 group-hover:drop-shadow-[0_0_12px_rgba(255,255,255,0.25)]" style={{ background: `radial-gradient(circle, ${c.glowColor} 0%, ${c.glowColor.replace(/[\d\.]+\)$/, '0.4)')} 30%, transparent 70%)` }}>
                  <div className={`p-2 rounded-full flex items-center justify-center text-white`}>{c.icon}</div>
                </div>
                <span className="text-[11px] font-bold text-slate-300 tracking-widest uppercase group-hover:text-white transition-colors">{c.name}</span>
              </button>
            ))}
          </div>
          <div className="w-full relative z-10 mb-8 flex flex-col gap-4">
            <button onClick={() => { setFilterChar(null); setView('main'); }} className="w-full py-5 bg-white/[0.015] border border-white/5 rounded-full flex items-center justify-center gap-4 group overflow-hidden relative text-center shadow-[0_0_20px_rgba(255,255,255,0.02)]">
              <span className="text-[12px] font-bold tracking-[0.6em] text-white/40 group-hover:text-white/80 uppercase">進入紀錄</span>
            </button>
            <div className="flex flex-col items-center gap-4">
              <div className="flex justify-center items-center gap-2 text-[11px] font-bold text-slate-700 uppercase tracking-[0.2em] mb-6">
                  {user ? <Cloud size={10} className="text-emerald-500/50" /> : <CloudOff size={10} />}
                  {user ? '雲端同步已開啟' : '離線模式'}
              </div>
              <div className="flex items-center gap-6">
                <button onClick={handleExport} className="flex items-center gap-2 text-slate-500 hover:text-indigo-400 transition-colors group">
                  <Download size={14} className="group-hover:scale-110 transition-transform" />
                  <span className="text-[10px] font-black tracking-[0.2em] uppercase">匯出資料</span>
                </button>
                <div className="w-px h-2 bg-slate-800"></div>
                <label className="flex items-center gap-2 text-slate-500 hover:text-amber-400 transition-colors group cursor-pointer">
                  <Upload size={14} className="group-hover:scale-110 transition-transform" />
                  <span className="text-[10px] font-black tracking-[0.2em] uppercase">匯入資料</span>
                  <input type="file" accept=".json" onChange={handleImport} className="hidden" />
                </label>
              </div>
            </div>
          </div>
          <div className="absolute bottom-6 flex items-center gap-3 opacity-40 hover:opacity-100 transition-opacity cursor-default">
             <CreatorCat size={18} className="text-indigo-300/60" />
             <span className="text-[10px] text-indigo-200 font-black tracking-[0.2em] uppercase">Yi.Fay.e</span>
          </div>
        </main>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-[#050508] text-slate-200 font-sans flex justify-center overflow-x-hidden">
      <style>{`
        .custom-scrollbar::-webkit-scrollbar { height: 3px; width: 3px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: rgba(255, 255, 255, 0.05); border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: rgba(255, 255, 255, 0.2); border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: rgba(255, 255, 255, 0.4); }
        .custom-scrollbar { scrollbar-width: thin; scrollbar-color: rgba(255, 255, 255, 0.2) rgba(255, 255, 255, 0.05); }
      `}</style>
      <main className="w-full max-w-md bg-black/50 backdrop-blur-3xl border-x border-white/5 relative z-10 min-h-screen flex flex-col shadow-2xl">
        <div className="absolute top-8 left-6 z-20">
            <button onClick={() => { if (filterChar) { setFilterChar(null); setSelectedEventFilter('all'); } else { setView('landing'); } }} className="p-2 bg-white/5 rounded-full hover:bg-white/10 transition-all text-slate-400"><ArrowLeft size={20} /></button>
        </div>

        <div className="pt-20 px-6 pb-2">
          <div className="flex justify-between items-center overflow-x-auto custom-scrollbar gap-4 text-center pb-3">
            {CHARACTERS.slice(0, 6).map(c => {
              const isActive = filterChar?.id === c.id;
              const currentGlow = isActive ? c.glowColor : c.dimGlow;
              return (
                <button key={c.id} onClick={() => {setFilterChar(c); setSelectedEventFilter('all');}} className={`group flex-shrink-0 flex flex-col items-center gap-1.5 transition-all duration-300 ${isActive ? 'scale-110' : 'opacity-85 hover:opacity-100'}`}>
                  <div className="w-14 h-14 rounded-full flex items-center justify-center transition-all duration-500 group-hover:brightness-125 group-hover:drop-shadow-[0_0_10px_rgba(255,255,255,0.25)]" style={{ background: `radial-gradient(circle, ${currentGlow} 0%, ${currentGlow.replace(/[\d\.]+\)$/, (isActive ? '0.5' : '0.25') + ')')} 40%, transparent 75%)` }}>
                     <div className={`p-2 rounded-full flex items-center justify-center transition-colors duration-500 ${isActive ? 'text-white' : 'text-slate-300/90'}`}>{c.icon}</div>
                  </div>
                  <span className={`text-[11px] font-black tracking-wider transition-colors duration-500 ${isActive ? 'text-white' : 'text-slate-500 group-hover:text-slate-200'}`}>{c.name}</span>
                </button>
              );
            })}
          </div>
        </div>

        <header className="pt-8 pb-4 px-8 flex justify-between items-center">
          <h1 className="text-2xl font-black tracking-[0.2em] text-white">{filterChar ? `${filterChar.name} 統計` : '許願紀錄'}</h1>
        </header>

        {filterChar ? (
          <div className="px-6 mb-8">
            <div className={`bg-gradient-to-br ${filterChar.solidColor} rounded-[2rem] pt-7 px-7 pb-3 text-white shadow-2xl relative overflow-hidden min-h-[160px] flex flex-col justify-center`}>
              <div className="absolute inset-0 bg-black/5 pointer-events-none"></div>
              <div className="relative z-10 flex justify-between items-start mb-2">
                <div>
                  <p className="text-[11px] font-black opacity-80 uppercase tracking-widest mb-1">{filterChar.engName}</p>
                  <h2 className="text-3xl font-black drop-shadow-lg">{filterChar.name}</h2>
                </div>
                <div className="text-right">
                  {filterChar.id !== 'other' && (
                    <div className="flex flex-col items-end">
                      <p className="text-[12px] font-black opacity-100 italic tracking-widest mb-1 text-white/95">與你見面</p>
                      <p className="text-5xl font-light drop-shadow-md leading-none">{history.filter(r => r.character === filterChar.id && r.recordType === 'pull').length}</p>
                    </div>
                  )}
                </div>
              </div>
              
              <div className="relative z-10 mt-3 border-t border-white/10 pt-3 flex items-center">
                <span className="absolute -top-1 -left-1 text-xl text-white/10 font-serif">“</span>
                <p className="text-[13px] font-medium italic tracking-tight text-white/90 leading-[1.2rem] max-w-full px-4 text-left w-full">
                   {activeQuote}
                </p>
              </div>
            </div>
          </div>
        ) : (
          <>
            <div className="px-6 mb-4">
              <div className="grid grid-cols-4 gap-1 p-1 bg-white/5 rounded-2xl border border-white/10">
                {MAIN_TABS.map(tab => (
                  <button key={tab.id} onClick={() => {setActiveTab(tab.id); setSelectedEventFilter('all');}} className={`py-3 rounded-xl transition-all text-[11px] font-black ${activeTab === tab.id ? 'bg-indigo-600 text-white shadow-lg' : 'text-slate-400 hover:text-slate-200'}`}>{tab.name}</button>
                ))}
              </div>
            </div>
            <div className="px-6 mb-6">
              <div className="bg-gradient-to-br from-slate-900/60 to-black/80 rounded-[2.5rem] p-8 border border-white/10 text-center relative shadow-2xl">
                <p className="text-[11px] text-slate-200 tracking-[0.4em] uppercase font-black mb-3 drop-shadow-md">距離保底剩餘</p>
                <div className="flex items-center justify-center gap-1">
                  <span className="text-6xl font-light tracking-tighter text-white drop-shadow-lg">{currentRemaining}</span>
                  <span className="text-xl text-slate-500 font-light opacity-80">/ 70</span>
                </div>
                <div className="mt-6 flex flex-col items-center gap-3">
                  <span className="px-4 py-1.5 rounded-full text-[11px] font-black border bg-white/10 border-white/20 text-slate-100 italic shadow-inner">已累積 {pityInfo.accumulated} 抽</span>
                  <div className="flex gap-2">
                    <div className={`px-3 py-1 rounded-full text-[11px] font-black tracking-widest border ${status.isGuaranteed ? 'bg-rose-500/30 border-rose-500/60 text-white shadow-sm' : 'bg-white/10 border-white/20 text-slate-200'}`}>{status.isGuaranteed ? '大保底' : '小保底'}</div>
                    {status.isTargetReady && <div className="px-3 py-1 rounded-full text-[11px] font-black tracking-widest border bg-amber-500/30 border-amber-500/60 text-white shadow-sm">定向中</div>}
                  </div>
                </div>
              </div>
            </div>
            <div className="px-6 mb-8 text-center">
              <button onClick={openWishModal} className="w-full py-4 bg-indigo-600 border border-indigo-400 rounded-2xl font-black text-xs text-white flex items-center justify-center gap-3 shadow-xl active:scale-95 transition-all"><Sparkles size={16} /> 登錄紀錄</button>
            </div>
          </>
        )}

        <div className="flex-1 bg-black/60 rounded-t-[3.5rem] border-t border-white/10 pt-10 px-8 pb-12 overflow-y-auto custom-scrollbar">
          <div className="flex items-center justify-between mb-8">
            <h2 className="text-xs font-black tracking-[0.3em] text-slate-500 uppercase flex items-center gap-2"><History size={14} />歷程</h2>
            {!filterChar && (
              <div className="relative group">
                <select value={selectedEventFilter} onChange={(e) => setSelectedEventFilter(e.target.value)} className="bg-white/5 border border-white/10 rounded-lg pl-8 pr-3 py-1.5 text-[11px] font-black text-slate-400 outline-none appearance-none hover:bg-white/10 transition-all">
                  <option value="all">全活動歷程</option>
                  {eventOptions.filter(opt => opt !== 'all').map(opt => <option key={opt} value={opt}>{opt}</option>)}
                </select>
                <Filter size={12} className="absolute left-2.5 top-1/2 -translate-y-1/2 text-slate-600 pointer-events-none" />
              </div>
            )}
          </div>

          {eventAnalysis && !filterChar && (
            <div className="mb-8 p-6 bg-[#131322] border border-white/10 rounded-[2rem] shadow-2xl relative overflow-hidden">
               <div className="flex items-center gap-3 mb-6">
                  <div className="p-2 bg-indigo-500/20 rounded-xl text-indigo-400"><BarChart3 size={18} /></div>
                  <div>
                    <h3 className="text-sm font-black text-white tracking-widest">許願紀錄統計</h3>
                    <p className="text-[9px] text-slate-500 font-black uppercase tracking-widest">EVENT DATA ANALYSIS</p>
                  </div>
               </div>

               <div className="grid grid-cols-3 gap-3 mb-8">
                  <div className="bg-black/40 rounded-2xl p-4 border border-white/5 text-center flex flex-col justify-center">
                    <p className="text-[10px] font-black text-slate-500 mb-2">總抽數</p>
                    <p className="text-2xl font-black text-white">{eventAnalysis.totalPulls}</p>
                  </div>
                  <div className="bg-black/40 rounded-2xl p-4 border border-white/5 text-center flex flex-col justify-center">
                    <p className="text-[10px] font-black text-slate-500 mb-2">出金</p>
                    <p className="text-2xl font-black text-amber-500">{eventAnalysis.pullCount}</p>
                  </div>
                  <div className="bg-black/40 rounded-2xl p-4 border border-white/5 text-center flex flex-col justify-center">
                    <p className="text-[10px] font-black text-slate-500 mb-2">墊池</p>
                    <p className="text-2xl font-black text-indigo-400">{eventAnalysis.pityCount}</p>
                  </div>
               </div>

               <div>
                  <div className="flex items-center gap-2 mb-4 px-1">
                    <div className="w-4 h-4 rounded-full bg-amber-500/20 border border-amber-500/40 flex items-center justify-center">
                      <div className="w-1.5 h-1.5 rounded-full bg-amber-500"></div>
                    </div>
                    <p className="text-[11px] font-black text-slate-400 uppercase tracking-widest">出金分佈 (不計墊池)</p>
                  </div>
                  
                  <div className="space-y-3">
                    {eventAnalysis.distribution.length > 0 ? (
                      eventAnalysis.distribution.map((item, idx) => (
                        <div key={item.id} className="bg-black/20 rounded-2xl px-5 py-4 flex items-center justify-between border border-white/5">
                           <div className="flex items-center gap-3">
                              <span className="text-[11px] font-black text-slate-700">#{eventAnalysis.distribution.length - idx}</span>
                              <span className="text-[13px] font-black text-slate-200">{item.charName}</span>
                           </div>
                           <div className="flex items-center gap-2">
                              <span className="bg-indigo-500/10 text-indigo-400 px-3 py-1 rounded-full text-[10px] font-black italic">累計第 {item.accumulated} 抽</span>
                              <span className="text-amber-500 font-black text-[11px]">({item.cost} 抽)</span>
                           </div>
                        </div>
                      ))
                    ) : (
                      <p className="text-[10px] text-slate-600 italic text-center py-4">目前尚無出金紀錄</p>
                    )}
                  </div>
               </div>
            </div>
          )}

          <div className="space-y-4">
            {currentItems.map((record) => {
              const charObj = CHARACTERS.find(c => c.id === record.character) || CHARACTERS[5];
              const isPity = record.recordType === 'pity';
              const label = isPity ? (record.pityLabel || '墊池') : '出金';
              const isMultiPool = record.subPoolType === 'multi';
              const showTargetTag = isMultiPool && !record.isLost5050 && !record.isNotTarget && !isPity;
              const showNotTargetTag = record.isNotTarget && !isPity;

              return (
                <div key={record.id} onClick={() => openEdit(record)} className={`rounded-3xl p-5 border transition-all relative overflow-hidden active:scale-[0.98] cursor-pointer group ${isPity ? 'bg-slate-900/40 border-slate-700/50' : 'bg-gradient-to-br from-amber-500/10 to-transparent border-amber-500/40'}`}>
                  <div className="absolute top-4 right-4 z-20 flex gap-2">
                    <button onClick={(e) => { e.stopPropagation(); openEdit(record); }} className="p-1.5 bg-white/10 hover:bg-white/20 rounded-lg text-slate-300 shadow-sm"><Edit2 size={14} /></button>
                    <button onClick={(e) => deleteRecord(e, record.id)} className="p-1.5 bg-rose-500/10 hover:bg-rose-500/20 rounded-lg text-rose-400 shadow-sm"><Trash2 size={14} /></button>
                  </div>
                  <div className="flex justify-between items-start mb-3 relative z-10">
                     <div className="flex items-center gap-2">
                        <span className={`text-[11px] font-black uppercase px-2 py-0.5 rounded ${isPity ? 'bg-indigo-900/40 text-indigo-300' : 'bg-amber-500/20 text-amber-500'}`}>{label}</span>
                        <span className="text-[11px] font-black text-slate-400 uppercase">{record.poolType === 'event' ? (record.subPoolType === 'multi' ? '限定(混)' : '限定(單)') : MAIN_TABS.find(t=>t.id===record.poolType)?.name}</span>
                     </div>
                     <span className="text-[11px] text-slate-500 font-bold flex items-center gap-1 mr-16"><Clock size={10}/> {record.eventDate}</span>
                  </div>
                  <div className="flex items-center gap-4 relative z-10">
                    <div className={`w-10 h-10 rounded-2xl bg-gradient-to-br ${charObj.solidColor} flex items-center justify-center shadow-lg relative text-white`}>{charObj.icon}</div>
                    <div className="flex-1 overflow-hidden">
                      <div className="flex wrap items-center gap-2 mb-1">
                         <span className="font-black text-sm text-white">{charObj.name}</span>
                         <span className={`text-[11px] font-black px-1.5 rounded ${isPity ? 'text-slate-400 bg-slate-800' : 'text-black bg-amber-400'}`}>{isPity ? `${record.totalPulls} 抽 (剩 ${record.remainingAtRecord})` : `第 ${record.totalPulls} 抽`}</span>
                         {record.isLost5050 && <span className="text-[9px] bg-rose-500/20 text-rose-400 px-1.5 py-0.5 rounded font-black italic">歪卡</span>}
                         {showNotTargetTag && <span className="text-[9px] bg-sky-500/10 text-sky-400 px-1.5 py-0.5 rounded font-black italic border border-sky-500/20">非定向</span>}
                         {showTargetTag && <span className="text-[9px] bg-amber-500/10 text-amber-400 px-1.5 py-0.5 rounded font-black italic border border-amber-500/30 flex items-center gap-0.5"><Crown size={8} fill="currentColor" /> 定向</span>}
                      </div>
                      <div className="flex items-center gap-1 mb-1"><Tag size={10} className="text-indigo-400 opacity-70" /><p className="text-[11px] font-black text-indigo-400/80 tracking-wider truncate uppercase">{record.eventName || '一般活動'}</p></div>
                      <div className="flex items-center gap-1.5">{record.cardType === 'sun' ? <Sun size={12} className="text-red-500" /> : <Moon size={12} className="text-sky-400" />}<p className="text-[14px] font-bold text-slate-100 truncate leading-tight">{record.cardName}</p></div>
                    </div>
                  </div>
                </div>
              );
            })}
          </div>

          {totalPages > 1 && (
            <div className="mt-10 flex items-center justify-center gap-3">
              <button onClick={() => setCurrentPage(prev => Math.max(prev - 1, 1))} disabled={currentPage === 1} className={`p-2 rounded-xl ${currentPage === 1 ? 'text-slate-800 opacity-20' : 'text-slate-400 bg-white/5 active:scale-90'}`}><ChevronLeft size={16} /></button>
              <div className="flex items-center gap-1">
                {Array.from({ length: totalPages }, (_, i) => i + 1).map(page => (
                  <button key={page} onClick={() => setCurrentPage(page)} className={`w-8 h-8 rounded-lg text-[11px] font-black ${currentPage === page ? 'bg-indigo-600 text-white shadow-lg' : 'text-slate-600 hover:text-slate-400'}`}>{page}</button>
                ))}
              </div>
              <button onClick={() => setCurrentPage(prev => Math.min(prev + 1, totalPages))} disabled={currentPage === totalPages} className={`p-2 rounded-xl ${currentPage === totalPages ? 'text-slate-800 opacity-20' : 'text-slate-400 bg-white/5 active:scale-90'}`}><ChevronRight size={16} /></button>
            </div>
          )}
        </div>
      </main>

      {isModalOpen && (
        <div className="fixed inset-0 z-[100] flex items-end justify-center bg-black/90 backdrop-blur-md p-4">
          <div className="absolute inset-0" onClick={resetModal}></div>
          <div className="bg-[#0c0c16] border border-white/10 rounded-[3rem] w-full max-w-sm relative p-8 shadow-2xl overflow-y-auto max-h-[90vh] custom-scrollbar">
            <button onClick={resetModal} className="absolute left-6 top-8 text-slate-500 hover:text-white"><ChevronLeft size={24} /></button>
            <h3 className="text-lg font-black text-white tracking-widest mb-8 text-center mt-6">登錄思念紀錄</h3>
            <div className="space-y-6">
                {activeTab === 'event' && (
                  <div>
                    <label className="text-[10px] text-slate-500 font-black uppercase mb-3 block tracking-widest text-center">限定池類型</label>
                    <div className="grid grid-cols-2 gap-3 p-1 bg-white/5 rounded-2xl border border-white/10">
                      <button onClick={() => setSubPoolType('single')} className={`flex items-center justify-center gap-2 py-2.5 rounded-xl text-[11px] font-black ${subPoolType === 'single' ? 'bg-indigo-600 text-white' : 'text-slate-500'}`}><User size={12} /> 單人池</button>
                      <button onClick={() => setSubPoolType('multi')} className={`flex items-center justify-center gap-2 py-2.5 rounded-xl text-[11px] font-black ${subPoolType === 'multi' ? 'bg-indigo-600 text-white' : 'text-slate-500'}`}><Users size={12} /> 多人混池</button>
                    </div>
                  </div>
                )}
                <div>
                  <label className="text-[10px] text-slate-500 font-black uppercase mb-3 block tracking-widest text-center">紀錄性質</label>
                  <div className="grid grid-cols-2 gap-3 p-1 bg-white/5 rounded-2xl">
                    <button onClick={() => setRecordType('pull')} className={`flex items-center justify-center gap-2 py-3 rounded-xl text-[12px] font-black ${recordType === 'pull' ? 'bg-amber-500 text-black shadow-lg' : 'text-slate-500'}`}><Target size={14} /> 岀金</button>
                    <button onClick={() => setRecordType('pity')} className={`flex items-center justify-center gap-2 py-3 rounded-xl text-[12px] font-black ${recordType === 'pity' ? 'bg-indigo-600 text-white shadow-lg' : 'text-slate-500'}`}><Zap size={14} /> 墊池</button>
                  </div>
                </div>
                <div>
                    <label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest">思念角色</label>
                    <div className="grid grid-cols-3 gap-2">
                        {CHARACTERS.map(c => <button key={c.id} onClick={() => setSelectedChar(c.id)} className={`py-3 rounded-xl border text-[10px] font-black transition-all ${selectedChar === c.id ? 'bg-white/10 border-white/40 text-white shadow-lg' : 'bg-white/5 border-transparent text-slate-400'}`}>{c.name}</button>)}
                    </div>
                </div>
                <div><label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest">活動名稱</label><input type="text" list="existing-events" placeholder="例：他的邀約" value={eventName} onChange={(e) => setEventName(e.target.value)} className="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-[12px] text-slate-200 focus:outline-none" /><datalist id="existing-events">{eventOptions.filter(opt => opt !== 'all').map(opt => <option key={opt} value={opt} />)}</datalist></div>
                <div className="flex gap-4 items-end">
                  <div className="w-1/3 flex-shrink-0"><label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest">活動日期</label><input type="date" value={eventDate} onChange={(e) => setEventDate(e.target.value)} className="w-full bg-white/5 border border-white/10 rounded-xl px-2 py-3 text-[11px] text-slate-200 focus:outline-none [color-scheme:dark]" /></div>
                  <div className="flex-1 flex flex-col items-center"><label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest text-center">類型</label><div className="flex gap-2 p-1 bg-white/5 rounded-xl border border-white/10 w-fit"><button onClick={() => setCardType('sun')} className={`px-3 py-2 rounded-lg transition-all flex items-center gap-1.5 text-[10px] font-black ${cardType === 'sun' ? 'bg-amber-500 text-black shadow-lg' : 'text-slate-500'}`}><Sun size={12} /> 日卡</button><button onClick={() => setCardType('moon')} className={`px-3 py-2 rounded-lg transition-all flex items-center gap-1.5 text-[10px] font-black ${cardType === 'moon' ? 'bg-sky-500 text-white shadow-lg' : 'text-slate-500'}`}><Moon size={12} /> 月卡</button></div></div>
                </div>
                <div><label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest">思念全名</label><input type="text" placeholder="例如：沈星回．心緒捕捉" value={cardName} onChange={(e) => setCardName(e.target.value)} className="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-[12px] text-slate-200 focus:outline-none" /></div>
                {recordType === 'pull' && activeTab !== 'standard' && (
                  <div className="space-y-4">
                    <div className="flex items-center justify-between p-4 bg-rose-500/5 rounded-2xl border border-rose-500/20"><div><span className="text-[12px] font-black text-rose-300 block">歪卡？</span></div><button onClick={() => { setIsLost5050(!isLost5050); if(!isLost5050) setIsNotTarget(false); }} className={`px-4 py-1.5 rounded-full text-[11px] font-black ${isLost5050 ? 'bg-rose-500 text-white shadow-lg' : 'text-slate-400'}`}>{isLost5050 ? '是的' : '沒歪'}</button></div>
                    {subPoolType === 'multi' && !isLost5050 && (<div className="flex items-center justify-between p-4 bg-sky-500/5 rounded-2xl border border-sky-500/20"><div><span className="text-[12px] font-black text-sky-400 block">非定向限定卡？</span></div><button onClick={() => setIsNotTarget(!isNotTarget)} className={`px-4 py-1.5 rounded-full text-[11px] font-black ${isNotTarget ? 'bg-sky-500 text-white shadow-lg' : 'text-slate-400'}`}>{isNotTarget ? '是的' : '定向中'}</button></div>)}
                  </div>
                )}
                <div className="pt-4 border-t border-white/5 space-y-4">
                  <div><label className="text-[10px] text-slate-500 font-black uppercase mb-3 block tracking-widest text-center">選擇填寫方式</label><div className="relative"><select value={inputMode} onChange={(e) => setInputMode(e.target.value)} className="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-[12px] font-black text-slate-200 outline-none appearance-none"><option value="cost">花費抽數</option><option value="remain">剩餘抽數</option></select><ChevronDown size={14} className="absolute right-4 top-1/2 -translate-y-1/2 text-slate-500" /></div></div>
                  <div><label className="text-[10px] text-slate-500 font-black uppercase mb-2 block tracking-widest">{inputMode === 'cost' ? '花費抽數' : '剩餘抽數'}</label><input type="number" value={inputMode === 'cost' ? pullCountInput : remainingPullsInput} onChange={(e) => inputMode === 'cost' ? setPullCountInput(e.target.value) : setRemainingPullsInput(e.target.value)} className="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-[12px] text-slate-200 focus:outline-none" /></div>
                </div>
                <button onClick={submitRecord} className="w-full py-5 rounded-full bg-indigo-600 text-white font-black text-[11px] uppercase tracking-[0.3em] shadow-xl active:scale-95">確認登錄</button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

