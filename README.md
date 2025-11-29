<html lang="th">
  <head>
    <meta charset="UTF-8" />
    <title>Ratchaburi PLC Hub - ชุมชนการเรียนรู้ทางวิชาชีพ</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link
      href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap"
      rel="stylesheet"
    />

    <script>
      tailwind.config = {
        theme: {
          extend: {
            fontFamily: {
              sans: ['Sarabun', 'sans-serif'],
            },
            colors: {
              primary: '#1e3a8a', // Dark Blue
              secondary: '#fbbf24', // Amber/Yellow
              accent: '#3b82f6',
            },
          },
        },
      };
    </script>

    <!-- React + ReactDOM + Babel (สำหรับใช้ JSX ในไฟล์เดียว) -->
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
      body {
        font-family: 'Sarabun', system-ui, -apple-system, BlinkMacSystemFont,
          'Segoe UI', sans-serif;
      }
      /* simple fade-in */
      .animate-fade-in {
        animation: fade-in 0.4s ease-out;
      }
      @keyframes fade-in {
        from {
          opacity: 0;
          transform: translateY(4px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }
    </style>
  </head>

  <body class="bg-slate-50 text-slate-900">
    <div id="root"></div>

    <script type="text/babel">
      const { useState, useEffect } = React;

      const SUBJECT_ICONS = {
        THAI: '📚',
        MATH: '📐',
        SCIENCE: '🧬',
        SOCIAL: '🌍',
        HEALTH: '⚽',
        ART: '🎨',
        CAREER: '🔨',
        FOREIGN: '🗣️',
        ACTIVITY: '🌱',
        ADMIN: '👔',
      };

      const MOCK_RESOURCES = [
        {
          id: '1',
          title: 'แผนการสอน Active Learning เรื่อง ตรรกศาสตร์',
          author: 'ครูสมชาย ใจดี',
          subjectArea: 'MATH',
          type: 'LESSON_PLAN',
          date: '2023-10-15',
          downloads: 120,
          description:
            'แผนการจัดการเรียนรู้ที่เน้นผู้เรียนเป็นสำคัญ โดยใช้เกมเป็นฐาน',
        },
        {
          id: '2',
          title: 'ชุดฝึกทักษะการอ่านจับใจความภาษาไทย',
          author: 'ครูสมหญิง รักเรียน',
          subjectArea: 'THAI',
          type: 'INNOVATION',
          date: '2023-11-02',
          downloads: 85,
          description:
            'นวัตกรรมชุดฝึกทักษะเพื่อแก้ปัญหาการอ่านจับใจความสำหรับนักเรียนชั้น ม.1',
        },
        {
          id: '3',
          title:
            'การเปรียบเทียบผลสัมฤทธิ์ทางการเรียนวิชาวิทยาศาสตร์',
          author: 'ครูวิทยา ช่างคิด',
          subjectArea: 'SCIENCE',
          type: 'CLASSROOM_RESEARCH',
          date: '2023-09-20',
          downloads: 45,
          description: 'งานวิจัยในชั้นเรียนเรื่องการใช้สื่อ AR ในการสอนระบบสุริยะ',
        },
        {
          id: '4',
          title: 'Role Play for English Conversation',
          author: 'Teacher John Doe',
          subjectArea: 'FOREIGN',
          type: 'LESSON_PLAN',
          date: '2023-12-05',
          downloads: 200,
          description:
            'แผนการสอนภาษาอังกฤษเพื่อการสื่อสารผ่านบทบาทสมมติ',
        },
        {
          id: '5',
          title: 'รูปแบบการบริหารสถานศึกษาสู่ความเป็นเลิศ',
          author: 'ผอ. มั่นคง',
          subjectArea: 'ADMIN',
          type: 'INNOVATION',
          date: '2023-11-15',
          downloads: 300,
          description:
            'โมเดลการบริหารงานวิชาการเพื่อยกระดับผลสัมฤทธิ์ทางการเรียนระดับเขตพื้นที่',
        },
      ];

      function Layout({ user, onLogout, activeTab, setActiveTab, children }) {
        return (
          <div className="min-h-screen flex flex-col">
            {/* Top bar */}
            <header className="bg-primary text-white shadow">
              <div className="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
                <div className="flex items-center gap-3">
                  <div className="w-9 h-9 rounded-xl bg-secondary flex items-center justify-center text-primary text-xl font-bold">
                    PLC
                  </div>
                  <div>
                    <h1 className="font-semibold text-lg">
                      Ratchaburi PLC Hub
                    </h1>
                    <p className="text-xs text-blue-100">
                      ชุมชนการเรียนรู้ทางวิชาชีพ สพม.ราชบุรี
                    </p>
                  </div>
                </div>

                {user && (
                  <div className="flex items-center gap-3 text-sm">
                    <div className="text-right">
                      <div className="font-medium">{user.name}</div>
                      <div className="text-blue-100 text-xs">
                        {user.roleLabel} · {user.subjectAreaLabel}
                      </div>
                    </div>
                    <button
                      onClick={onLogout}
                      className="px-3 py-1.5 rounded-full bg-white/10 hover:bg-white/20 text-xs"
                    >
                      ออกจากระบบ
                    </button>
                  </div>
                )}
              </div>
            </header>

            {/* Tabs */}
            {user && (
              <nav className="bg-white border-b border-slate-200">
                <div className="max-w-6xl mx-auto px-4">
                  <div className="flex gap-4">
                    {[
                      { id: 'dashboard', label: 'แดชบอร์ด' },
                      {
                        id: 'plans',
                        label: 'แผนการจัดการเรียนรู้',
                      },
                      { id: 'innovations', label: 'นวัตกรรมการเรียนรู้' },
                      { id: 'research', label: 'งานวิจัยในชั้นเรียน' },
                    ].map((tab) => (
                      <button
                        key={tab.id}
                        onClick={() => setActiveTab(tab.id)}
                        className={
                          'py-3 text-sm border-b-2 -mb-px transition-colors px-1' +
                          (activeTab === tab.id
                            ? ' border-primary text-primary font-semibold'
                            : ' border-transparent text-slate-500 hover:text-primary')
                        }
                      >
                        {tab.label}
                      </button>
                    ))}
                  </div>
                </div>
              </nav>
            )}

            {/* Content */}
            <main className="flex-1">
              <div className="max-w-6xl mx-auto px-4 py-6">{children}</div>
            </main>

            <footer className="border-t border-slate-200 bg-white">
              <div className="max-w-6xl mx-auto px-4 py-3 text-xs text-slate-500 flex justify-between">
                <span>© {new Date().getFullYear()} Ratchaburi PLC Hub</span>
                <span>พัฒนาเพื่อสนับสนุนชุมชนการเรียนรู้ทางวิชาชีพ (PLC)</span>
              </div>
            </footer>
          </div>
        );
      }

      function Welcome({ onLogin }) {
        const [name, setName] = useState('');
        const [role, setRole] = useState('Teacher');
        const [subject, setSubject] = useState('HEALTH');

        const roleLabel = {
          Teacher: 'ครู',
          Administrator: 'ผู้บริหารสถานศึกษา',
          Supervisor: 'ศึกษานิเทศก์',
          Other: 'ผู้เชี่ยวชาญ/อื่น ๆ',
        }[role];

        const subjectLabelMap = {
          THAI: 'ภาษาไทย',
          MATH: 'คณิตศาสตร์',
          SCIENCE: 'วิทยาศาสตร์',
          SOCIAL: 'สังคมศึกษา',
          HEALTH: 'สุขศึกษาและพลศึกษา',
          ART: 'ศิลปะ',
          CAREER: 'การงานอาชีพ',
          FOREIGN: 'ภาษาต่างประเทศ',
          ACTIVITY: 'กิจกรรมพัฒนาผู้เรียน',
          ADMIN: 'บริหารการศึกษา',
        };

        function handleSubmit(e) {
          e.preventDefault();
          if (!name.trim()) return;
          onLogin({
            name,
            role,
            roleLabel,
            subjectArea: subject,
            subjectAreaLabel: subjectLabelMap[subject],
          });
        }

        return (
          <div className="max-w-3xl mx-auto mt-8 grid md:grid-cols-2 gap-8 items-start">
            <div className="space-y-4">
              <div className="inline-flex items-center gap-2 rounded-full bg-blue-100 text-primary text-xs font-medium px-3 py-1">
                <span>✨ New</span>
                <span>แพลตฟอร์มแลกเปลี่ยนแผนการสอนและวิจัย</span>
              </div>
              <h2 className="text-3xl font-bold text-slate-800 leading-snug">
                ยินดีต้อนรับสู่
                <span className="text-primary block">
                  Ratchaburi PLC Hub
                </span>
              </h2>
              <p className="text-slate-600 text-sm">
                พื้นที่กลางสำหรับครูและบุคลากรทางการศึกษาในสังกัด
                สพม.ราชบุรี เพื่อแบ่งปันแผนการสอน นวัตกรรม และงานวิจัยในชั้นเรียน
                พร้อมสร้างชุมชนการเรียนรู้ทางวิชาชีพอย่างยั่งยืน
              </p>
              <ul className="text-sm text-slate-600 space-y-2">
                <li>• คลังแผนการสอนแยกตามกลุ่มสาระ</li>
                <li>• แหล่งรวมนวัตกรรมและ Best Practice</li>
                <li>• ตัวอย่างงานวิจัยในชั้นเรียนจากโรงเรียนต่าง ๆ</li>
              </ul>
            </div>

            <form
              onSubmit={handleSubmit}
              className="bg-white border border-slate-200 rounded-2xl p-6 shadow-sm space-y-4"
            >
              <h3 className="text-lg font-semibold text-slate-800 mb-1">
                ลงชื่อเข้าใช้งาน (Demo)
              </h3>
              <p className="text-xs text-slate-500 mb-3">
                ข้อมูลนี้ใช้ในระบบตัวอย่างเท่านั้น
              </p>

              <div className="space-y-1 text-sm">
                <label className="block font-medium text-slate-700">
                  ชื่อ - สกุล
                </label>
                <input
                  className="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-primary/40"
                  placeholder="ระบุชื่อที่ใช้แสดงในระบบ"
                  value={name}
                  onChange={(e) => setName(e.target.value)}
                />
              </div>

              <div className="space-y-1 text-sm">
                <label className="block font-medium text-slate-700">
                  บทบาท
                </label>
                <select
                  className="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-primary/40"
                  value={role}
                  onChange={(e) => setRole(e.target.value)}
                >
                  <option value="Teacher">ครู</option>
                  <option value="Administrator">ผู้บริหารสถานศึกษา</option>
                  <option value="Supervisor">ศึกษานิเทศก์</option>
                  <option value="Other">ผู้เชี่ยวชาญ/อื่น ๆ</option>
                </select>
              </div>

              <div className="space-y-1 text-sm">
                <label className="block font-medium text-slate-700">
                  กลุ่มสาระการเรียนรู้หลักของท่าน
                </label>
                <select
                  className="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-primary/40"
                  value={subject}
                  onChange={(e) => setSubject(e.target.value)}
                >
                  <option value="THAI">ภาษาไทย</option>
                  <option value="MATH">คณิตศาสตร์</option>
                  <option value="SCIENCE">วิทยาศาสตร์</option>
                  <option value="SOCIAL">สังคมศึกษา</option>
                  <option value="HEALTH">สุขศึกษาและพลศึกษา</option>
                  <option value="ART">ศิลปะ</option>
                  <option value="CAREER">การงานอาชีพ</option>
                  <option value="FOREIGN">ภาษาต่างประเทศ</option>
                  <option value="ACTIVITY">กิจกรรมพัฒนาผู้เรียน</option>
                  <option value="ADMIN">บริหารการศึกษา</option>
                </select>
              </div>

              <button
                type="submit"
                className="w-full mt-2 rounded-lg bg-primary text-white text-sm font-semibold py-2.5 hover:bg-blue-900 transition-colors"
              >
                เข้าสู่ระบบ
              </button>
            </form>
          </div>
        );
      }

      function ResourceList({ type, userSubject }) {
        const typeLabelMap = {
          LESSON_PLAN: 'แผนการจัดการเรียนรู้',
          INNOVATION: 'นวัตกรรมการเรียนรู้',
          CLASSROOM_RESEARCH: 'งานวิจัยในชั้นเรียน',
        };

        const filtered = MOCK_RESOURCES.filter((r) => r.type === type);

        return (
          <div className="space-y-4 animate-fade-in">
            <div className="flex flex-wrap items-center justify-between gap-3">
              <div>
                <h2 className="text-xl font-bold text-slate-800">
                  {typeLabelMap[type]}
                </h2>
                <p className="text-xs text-slate-500">
                  แสดงข้อมูลตัวอย่างจากคลังทรัพยากร (Demo)
                </p>
              </div>
              <div className="flex items-center gap-2 text-xs text-slate-600">
                <span className="px-2 py-1 rounded-full bg-slate-100">
                  กลุ่มสาระของคุณ:{' '}
                  <strong className="ml-1">{userSubject}</strong>
                </span>
              </div>
            </div>

            <div className="grid md:grid-cols-2 gap-4">
              {filtered.map((r) => (
                <div
                  key={r.id}
                  className="bg-white rounded-xl border border-slate-200 p-4 flex flex-col justify-between"
                >
                  <div className="flex items-start gap-3">
                    <div className="text-2xl">
                      {SUBJECT_ICONS[r.subjectArea] || '📁'}
                    </div>
                    <div className="space-y-1">
                      <h3 className="text-sm font-semibold text-slate-800">
                        {r.title}
                      </h3>
                      <p className="text-xs text-slate-500">
                        โดย {r.author} · เผยแพร่ {r.date}
                      </p>
                      <p className="text-xs text-slate-600">
                        {r.description}
                      </p>
                    </div>
                  </div>
                  <div className="mt-3 flex items-center justify-between text-xs text-slate-500">
                    <span>ดาวน์โหลดแล้ว {r.downloads.toLocaleString()} ครั้ง</span>
                    <button className="px-3 py-1 rounded-full border border-slate-300 hover:border-primary hover:text-primary transition-colors">
                      ดูรายละเอียด
                    </button>
                  </div>
                </div>
              ))}
            </div>
          </div>
        );
      }

      function Dashboard({ user, setActiveTab }) {
        return (
          <div className="space-y-8 animate-fade-in">
            <div className="bg-gradient-to-r from-secondary/20 to-orange-100 p-6 rounded-xl border border-secondary/30">
              <h2 className="text-2xl font-bold text-slate-800 mb-2">
                ยินดีต้อนรับ, {user.name}
              </h2>
              <p className="text-slate-600 text-sm">
                เข้าสู่ระบบในฐานะ:{' '}
                <span className="font-semibold">{user.roleLabel}</span> | กลุ่มสาระ:{' '}
                <span className="font-semibold">
                  {user.subjectAreaLabel}
                </span>
              </p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <div
                onClick={() => setActiveTab('plans')}
                className="bg-white border border-slate-200 rounded-xl p-6 hover:shadow-lg transition-all cursor-pointer group"
              >
                <div className="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center mb-4 group-hover:bg-blue-600 transition-colors">
                  <span className="text-2xl group-hover:scale-110 transition-transform">
                    📝
                  </span>
                </div>
                <h3 className="text-lg font-bold text-slate-800 mb-2">
                  แผนการจัดการเรียนรู้
                </h3>
                <p className="text-sm text-slate-500">
                  คลังแผนการสอนและสื่อการสอนจากครูในสังกัด สพม.ราชบุรี
                </p>
              </div>

              <div
                onClick={() => setActiveTab('innovations')}
                className="bg-white border border-slate-200 rounded-xl p-6 hover:shadow-lg transition-all cursor-pointer group"
              >
                <div className="w-12 h-12 bg-yellow-100 rounded-lg flex items-center justify-center mb-4 group-hover:bg-yellow-500 transition-colors">
                  <span className="text-2xl group-hover:scale-110 transition-transform">
                    💡
                  </span>
                </div>
                <h3 className="text-lg font-bold text-slate-800 mb-2">
                  นวัตกรรมการเรียนรู้
                </h3>
                <p className="text-sm text-slate-500">
                  แหล่งรวม Best Practice และนวัตกรรมเพื่อการศึกษา
                </p>
              </div>

              <div
                onClick={() => setActiveTab('research')}
                className="bg-white border border-slate-200 rounded-xl p-6 hover:shadow-lg transition-all cursor-pointer group"
              >
                <div className="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center mb-4 group-hover:bg-green-600 transition-colors">
                  <span className="text-2xl group-hover:scale-110 transition-transform">
                    🔬
                  </span>
                </div>
                <h3 className="text-lg font-bold text-slate-800 mb-2">
                  งานวิจัยในชั้นเรียน
                </h3>
                <p className="text-sm text-slate-500">
                  เผยแพร่งานวิจัยและผลงานทางวิชาการเพื่อพัฒนาผู้เรียน
                </p>
              </div>

              <div className="bg-gradient-to-br from-primary to-blue-800 rounded-xl p-6 text-white relative overflow-hidden">
                <div className="relative z-10">
                  <div className="w-12 h-12 bg-white/20 rounded-lg flex items-center justify-center mb-4">
                    <span className="text-2xl">🤖</span>
                  </div>
                  <h3 className="text-lg font-bold mb-2">
                    AI สืบค้นงานวิจัยโลก (Demo)
                  </h3>
                  <p className="text-sm text-blue-100">
                    ส่วนนี้เป็น placeholder สำหรับเชื่อมต่อ AI ภายหลัง
                  </p>
                </div>
                <div className="absolute right-0 bottom-0 opacity-10 transform translate-x-1/4 translate-y-1/4">
                  <span className="text-9xl">⚛️</span>
                </div>
              </div>
            </div>

            <div className="mt-4">
              <h3 className="text-lg font-bold text-slate-800 mb-4 border-l-4 border-primary pl-3">
                ข่าวสารประชาสัมพันธ์ (ตัวอย่าง)
              </h3>
              <div className="bg-white border border-slate-200 rounded-lg divide-y divide-slate-100">
                {[1, 2, 3].map((i) => (
                  <div
                    key={i}
                    className="p-4 hover:bg-slate-50 transition-colors"
                  >
                    <div className="flex gap-3">
                      <div className="flex-shrink-0 w-2 h-2 mt-2 rounded-full bg-secondary"></div>
                      <div>
                        <p className="text-sm font-medium text-slate-800">
                          ขอเชิญเข้าร่วมอบรมเชิงปฏิบัติการ PLC ประจำปี 2567
                        </p>
                        <p className="text-xs text-slate-400 mt-1">
                          ประกาศเมื่อ: 2 ชั่วโมงที่แล้ว โดย กลุ่มนิเทศฯ
                        </p>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        );
      }

      function App() {
        const [user, setUser] = useState(() => {
          try {
            const saved = localStorage.getItem('plc_ratchaburi_user_demo');
            return saved ? JSON.parse(saved) : null;
          } catch (e) {
            return null;
          }
        });
        const [activeTab, setActiveTab] = useState('dashboard');

        useEffect(() => {
          try {
            if (user) {
              localStorage.setItem(
                'plc_ratchaburi_user_demo',
                JSON.stringify(user)
              );
            } else {
              localStorage.removeItem('plc_ratchaburi_user_demo');
            }
          } catch (e) {}
        }, [user]);

        const handleLogout = () => {
          setUser(null);
          setActiveTab('dashboard');
        };

        let content;
        if (!user) {
          content = <Welcome onLogin={setUser} />;
        } else {
          if (activeTab === 'dashboard') {
            content = (
              <Dashboard user={user} setActiveTab={setActiveTab} />
            );
          } else if (activeTab === 'plans') {
            content = (
              <ResourceList type="LESSON_PLAN" userSubject={user.subjectArea} />
            );
          } else if (activeTab === 'innovations') {
            content = (
              <ResourceList type="INNOVATION" userSubject={user.subjectArea} />
            );
          } else if (activeTab === 'research') {
            content = (
              <ResourceList
                type="CLASSROOM_RESEARCH"
                userSubject={user.subjectArea}
              />
            );
          } else {
            content = <div>Not found</div>;
          }
        }

        return (
          <Layout
            user={user}
            onLogout={handleLogout}
            activeTab={activeTab}
            setActiveTab={setActiveTab}
          >
            {content}
          </Layout>
        );
      }

      const root = ReactDOM.createRoot(document.getElementById('root'));
      root.render(<App />);
    </script>
  </body>
</html>
