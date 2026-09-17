import React, { useState, useEffect } from 'react';

// ==========================================
// SOURCE KNOWLEDGE DATA (Extracted from Provided Materials)
// ==========================================
const SOURCE_VOCABULARY = [
  {
    id: 'v1',
    korean: '네',
    pos: '감',
    english: 'yes',
    vietnamese: 'Vâng, đúng vậy',
    example: '선생님: 앤디 씨, 숙제했어요? 앤디: 네, 했어요.',
    synonym: '예',
    antonym: '아니요',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v2',
    korean: '다시',
    pos: '부',
    english: 'again',
    vietnamese: 'Lại, lần nữa',
    example: '다시 한 번 말씀해 주시겠어요?',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v3',
    korean: '단어',
    pos: '명',
    english: 'a word',
    vietnamese: 'Từ vựng, từ',
    example: '한국 말을 잘하려면 단어를 많이 알아야 해요.',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v4',
    korean: '듣다',
    pos: '동',
    english: 'to listen',
    vietnamese: 'Nghe',
    irregular: "'ㄷ' 불규칙",
    example: '잘 듣고 따라 하세요.',
    collocation: '-이/가 -을/를 듣다, -에게서 -을/를 듣다',
    nounForm: '듣기',
    unit: '02 교육 - 교실 용어',
    difficulty: 2
  },
  {
    id: 'v5',
    korean: '들리다',
    pos: '동',
    english: 'to be heard',
    vietnamese: 'Nghe thấy, được nghe',
    example: '너무 시끄러워서 선생님 목소리가 잘 안 들려요.',
    collocation: '-이/가 들리다',
    related: '듣다',
    unit: '02 교육 - 교실 용어',
    difficulty: 2
  },
  {
    id: 'v6',
    korean: '따라 하다',
    pos: '동',
    english: 'to follow, copy an action',
    vietnamese: 'Làm theo, nhại theo',
    example: '선생님 말을 잘 듣고 따라 해 보세요.',
    collocation: '-이/가 -을/를 따라(서) 하다',
    unit: '02 교육 - 교실 용어',
    difficulty: 2
  },
  {
    id: 'v7',
    korean: '뜻',
    pos: '명',
    english: 'meaning',
    vietnamese: 'Ý nghĩa',
    example: '이 단어의 뜻을 잘 모르겠어요. 설명해 주세요.',
    synonym: '의미',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v8',
    korean: '맞다',
    pos: '동',
    english: 'to be right, correct',
    vietnamese: 'Đúng, chính xác',
    example: '선생님: 왕핑 씨, 전화번호가 234-5678이에요? 왕핑: 네, 맞아요.',
    antonym: '틀리다',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v9',
    korean: '이해',
    pos: '명',
    english: 'understanding',
    vietnamese: 'Sự hiểu, thấu hiểu',
    example: '한국에 처음 왔을 때에는 수업 시간에 하나도 이해할 수 없었어요.',
    collocation: '-을/를 이해하다',
    verbForm: '이해하다',
    unit: '02 교육 - 교실 용어',
    difficulty: 2
  },
  {
    id: 'v10',
    korean: '읽다',
    pos: '동',
    english: 'to read',
    vietnamese: 'Đọc',
    example: '책 25쪽을 읽어 보세요.',
    collocation: '-을/를 읽다',
    nounForm: '읽기',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v11',
    korean: '자리',
    pos: '명',
    english: 'a seat, room',
    vietnamese: 'Chỗ ngồi, vị trí',
    example: '리사: 여기 누구 자리예요? 앤디: 폴 씨 자리예요.',
    tip: "'자리 있어요?' nghĩa là 'Tôi ngồi đây được không?' hoặc 'Còn chỗ trống không?'",
    unit: '02 교육 - 교실 용어',
    difficulty: 2
  },
  {
    id: 'v12',
    korean: '조용히',
    pos: '부',
    english: 'quietly',
    vietnamese: 'Trật tự, một cách yên lặng',
    example: '지금 시험을 보고 있어요. 조용히 해 주세요.',
    nounForm: '조용하다',
    unit: '02 교육 - 교실 용어',
    difficulty: 1
  },
  {
    id: 'v13',
    korean: '가르치다',
    pos: '동',
    english: 'to teach',
    vietnamese: 'Dạy, giảng dạy',
    example: '저는 한국에서 아이들한테 영어를 가르치고 있어요.',
    collocation: '-을/를 -에게 가르치다',
    antonym: '배우다',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v14',
    korean: '가지다',
    pos: '동',
    english: 'to bring, to have',
    vietnamese: 'Mang theo, có',
    example: '오늘 한국어 회화 책을 가지고 왔어요?',
    collocation: '-을/를 가지다, 가지고 가다/오다',
    shortForm: '갖다',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v15',
    korean: '결석',
    pos: '명',
    english: 'absence',
    vietnamese: 'Vắng mặt, nghỉ học',
    example: '선생님: 올가 씨, 어제 왜 결석했어요? 올가: 감기에 걸려서 못 왔어요.',
    antonym: '출석',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v16',
    korean: '모르다',
    pos: '동',
    english: 'to not know',
    vietnamese: 'Không biết',
    irregular: "'르' 불규칙",
    example: '모르는 단어는 선생님께 물어보세요.',
    antonym: '알다',
    unit: '02 교육 - 수업',
    difficulty: 1
  },
  {
    id: 'v17',
    korean: '물어보다',
    pos: '동',
    english: 'to ask',
    vietnamese: 'Hỏi, hỏi thử',
    example: '질문이 있으면 쉬는 시간에 물어보세요.',
    honorific: '여쭤보다',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v18',
    korean: '발음',
    pos: '명',
    english: 'pronunciation',
    vietnamese: 'Phát âm',
    example: '리에 씨는 한국말 발음이 정확해요.',
    collocation: '발음이 좋다/나쁘다, 발음이 정확하다',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v19',
    korean: '발표',
    pos: '명',
    english: 'presentation',
    vietnamese: 'Phát biểu, thuyết trình',
    example: '지금부터 한국 역사에 대해서 발표하겠습니다.',
    collocation: '-을/를 발표하다, -에 대해(서) 발표하다',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v20',
    korean: '숙제',
    pos: '명',
    english: 'homework',
    vietnamese: 'Bài tập về nhà',
    example: '요즘 학교 숙제가 너무 많아서 힘들어요.',
    collocation: '숙제를 내다 (giao bài tập)',
    unit: '02 교육 - 수업',
    difficulty: 1
  },
  {
    id: 'v21',
    korean: '쓰다',
    pos: '동',
    english: 'to write / taste bitter / use / wear',
    vietnamese: 'Viết / Đắng / Sử dụng / Đội mũ, đeo kính',
    irregular: "'으' 불규칙",
    example: '저는 매일 공책에 일기를 써요.',
    tip: '1) 편지를 쓰다 (viết) 2) 맛이 쓰다 (đắng) 3) 물건을 쓰다 (dùng) 4) 모자를 쓰다 (đội)',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v22',
    korean: '알아듣다',
    pos: '동',
    english: 'to understand what was said',
    vietnamese: 'Nghe hiểu',
    irregular: "'ㄷ' 불규칙",
    example: '안나 씨의 말은 너무 빨라서 알아듣기 힘들어요.',
    tip: "'알아듣다' = '듣고 이해하다' (Nghe và hiểu)",
    unit: '02 교육 - 수업',
    difficulty: 3
  },
  {
    id: 'v23',
    korean: '연습',
    pos: '명',
    english: 'practice',
    vietnamese: 'Luyện tập',
    example: '저는 한국어 듣기 연습을 하기 위해 영화를 봐요.',
    unit: '02 교육 - 수업',
    difficulty: 1
  },
  {
    id: 'v24',
    korean: '예습',
    pos: '명',
    english: 'preview of lessons',
    vietnamese: 'Chuẩn bị bài trước',
    example: '내일 학교에서 배울 부분을 예습했어요.',
    antonym: '복습 (Ôn tập)',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v25',
    korean: '자세하다',
    pos: '형',
    english: 'to be detailed',
    vietnamese: 'Chi tiết, tỉ mỉ',
    example: '자세한 설명을 들으면 쉽게 이해할 수 있어요.',
    adverbForm: '자세히',
    unit: '02 교육 - 수업',
    difficulty: 2
  },
  {
    id: 'v26',
    korean: '답하다',
    pos: '동',
    english: 'to answer',
    vietnamese: 'Trả lời',
    example: '다음 글을 읽고 질문에 맞게 답하십시오.',
    synonym: '대답하다',
    antonym: '묻다',
    unit: '02 교육 - 시험',
    difficulty: 2
  },
  {
    id: 'v27',
    korean: '문장',
    pos: '명',
    english: 'a sentence',
    vietnamese: 'Câu văn',
    example: '다음 단어를 사용하여 문장을 완성하십시오.',
    unit: '02 교육 - 시험',
    difficulty: 2
  },
  {
    id: 'v28',
    korean: '문제',
    pos: '명',
    english: 'a question, problem',
    vietnamese: 'Câu hỏi, bài tập, vấn đề',
    example: '이 문제는 너무 어려워서 잘 모르겠어요.',
    collocation: '문제를 풀다 (giải câu hỏi), 문제가 쉽다/어렵다',
    unit: '02 교육 - 시험',
    difficulty: 1
  },
  {
    id: 'v29',
    korean: '물음',
    pos: '명',
    english: 'a question',
    vietnamese: 'Câu hỏi (trong đề thi)',
    example: '다음 글을 읽고 물음에 답하십시오.',
    related: '묻다',
    unit: '02 교육 - 시험',
    difficulty: 2
  },
  {
    id: 'v30',
    korean: '빈칸',
    pos: '명',
    english: 'a blank space',
    vietnamese: 'Chỗ trống, ô trống',
    example: '빈칸에 들어갈 말을 쓰세요.',
    collocation: '빈칸을 채우다 (điền vào chỗ trống)',
    unit: '02 교육 - 시험',
    difficulty: 2
  },
  {
    id: 'v31',
    korean: '쉽다',
    pos: '형',
    english: 'to be easy',
    vietnamese: 'Dễ',
    irregular: "'ㅂ' 불규칙",
    example: '문제가 쉬워서 시험을 잘 봤어요.',
    antonym: '어렵다',
    unit: '02 교육 - 시험',
    difficulty: 1
  },
  {
    id: 'v32',
    korean: '알맞다',
    pos: '형',
    english: 'to be suitable, correct',
    vietnamese: 'Phù hợp, thích hợp',
    example: '빈칸에 알맞은 말을 고르세요.',
    unit: '02 교육 - 시험',
    difficulty: 2
  },
  {
    id: 'v33',
    korean: '어렵다',
    pos: '형',
    english: 'to be difficult',
    vietnamese: 'Khó',
    irregular: "'ㅂ' 불규칙",
    example: '이 책은 모르는 단어가 많아서 읽기가 어려워요.',
    antonym: '쉽다',
    unit: '02 교육 - 시험',
    difficulty: 1
  },
  {
    id: 'v34',
    korean: '잘못',
    pos: '명/부',
    english: 'a mistake / wrong',
    vietnamese: 'Lỗi, nhầm / Sai, nhầm lẫn',
    example: '답안지에 답을 잘못 썼어요.',
    tip: "'잘못하다' là phạm lỗi/làm sai (mistake). Còn '잘 못하다' là không giỏi/không thạo.",
    unit: '02 교육 - 시험',
    difficulty: 3
  },
  {
    id: 'v35',
    korean: '방학',
    pos: '명',
    english: 'school vacation',
    vietnamese: 'Kỳ nghỉ hè/đông (trường học)',
    example: '이번 방학 때 뭐 할 거예요?',
    tip: 'Ở trường học dùng 방학, ở công ty dùng 휴가 (kỳ nghỉ phép).',
    unit: '02 교육 - 학교',
    difficulty: 2
  },
  {
    id: 'v36',
    korean: '배우다',
    pos: '동',
    english: 'to learn',
    vietnamese: 'Học',
    example: '저는 2년 전부터 일본어를 배우기 시작했어요.',
    antonym: '가르치다',
    unit: '02 교육 - 학교',
    difficulty: 1
  },
  {
    id: 'v37',
    korean: '잘하다',
    pos: '동',
    english: 'to do well',
    vietnamese: 'Giỏi, làm tốt',
    example: '어떻게 하면 한국어를 잘할 수 있을까요?',
    antonym: '못하다',
    unit: '02 교육 - 학교',
    difficulty: 1
  },
  {
    id: 'v38',
    korean: '전공',
    pos: '명',
    english: 'major',
    vietnamese: 'Chuyên ngành',
    example: '제 전공은 경영학이에요.',
    collocation: '전공을 바꾸다, 전공 과목',
    unit: '02 교육 - 학교',
    difficulty: 2
  },
  {
    id: 'v39',
    korean: '졸업',
    pos: '명',
    english: 'graduation',
    vietnamese: 'Tốt nghiệp',
    example: '저는 작년에 대학을 졸업했어요.',
    antonym: '입학 (Nhập học)',
    unit: '02 교육 - 학교',
    difficulty: 2
  }
];

const SOURCE_COLLOCATIONS = [
  { id: 'c1', left: '밑줄을', right: '치다', vietnamese: 'Gạch chân dưới từ' },
  { id: 'c2', left: '시험을', right: '보다', vietnamese: 'Làm bài thi / Tham gia kỳ thi' },
  { id: 'c3', left: '답이', right: '틀리다', vietnamese: 'Đáp án bị sai' },
  { id: 'c4', left: '문제를', right: '풀다', vietnamese: 'Giải bài tập / Làm đề' },
  { id: 'c5', left: '숙제를', right: '내다', vietnamese: 'Giao bài tập về nhà' },
  { id: 'c6', left: '빈칸을', right: '채우다', vietnamese: 'Điền vào chỗ trống' }
];

const SOURCE_QUESTIONS = [
  {
    id: 'q1',
    type: 'synonym',
    passage: '<보기> 다음 ㉠ 질문에 알맞은 ㉡ 은/는 무엇입니까?',
    question: '1. ㉠과 바꿔 쓸 수 있는 단어는 무엇입니까?',
    options: ['1) 물음', '2) 답', '3) 점', '4) 보기'],
    correct: 0,
    explanation: "'질문' (câu hỏi) đồng nghĩa với '물음' (câu hỏi trong đề thi). (Nguồn: Let's Check p.95)",
    topic: 'Vocabulary',
    skill: 'READING',
    difficulty: 2
  },
  {
    id: 'q2',
    type: 'blank_fill',
    passage: '<보기> 다음 ㉠ 질문에 알맞은 ㉡ 은/는 무엇입니까?',
    question: '2. ㉡에 들어갈 알맞은 단어는 무엇입니까?',
    options: ['1) 밑줄', '2) 답', '3) 빈칸', '4) 내용'],
    correct: 2,
    explanation: "Vị trí ㉡ là khoảng trống cần điền nên dùng '빈칸' (chỗ trống). (Nguồn: Let's Check p.95)",
    topic: 'Vocabulary',
    skill: 'READING',
    difficulty: 2
  },
  {
    id: 'q3',
    type: 'nuance_choice',
    passage: '• 저와 제 동생은 성격이 아주 ㉠ _________.\n• 맞으면 O, ㉡ _________ X 하십시오.',
    question: '3. ㉠과 ㉡에 들어갈 말로 알맞은 것을 고르십시오.',
    options: [
      '1) ㉠ 다릅니다   ㉡ 틀리면',
      '2) ㉠ 다릅니다   ㉡ 다르면',
      '3) ㉠ 틀립니다   ㉡ 다르면',
      '4) ㉠ 틀립니다   ㉡ 틀리면'
    ],
    correct: 0,
    explanation: "Tính cách khác nhau dùng '다르다' (khác). Nếu sai đánh dấu X dùng '틀리다' (sai). -> Chọn 1. (Nguồn: Let's Check p.95)",
    topic: 'Grammar & Nuance',
    skill: 'READING',
    difficulty: 3
  },
  {
    id: 'q4',
    type: 'collocation',
    question: "'시험을 ____'에 들어갈 알맞은 동사는 무엇입니까?",
    options: ['1) 치다', '2) 보다', '3) 풀다', '4) 내다'],
    correct: 1,
    explanation: "Cụm từ chuẩn TOPIK: '시험을 보다' (Tham gia kỳ thi).",
    topic: 'Collocation',
    skill: 'VOCABULARY',
    difficulty: 1
  },
  {
    id: 'q5',
    type: 'collocation',
    question: "'문제를 ____'에 들어갈 알맞은 동사는 무엇입니까?",
    options: ['1) 보다', '2) 치다', '3) 풀다', '4) 틀리다'],
    correct: 2,
    explanation: "Cụm từ chuẩn TOPIK: '문제를 풀다' (Giải câu hỏi / Làm bài tập).",
    topic: 'Collocation',
    skill: 'VOCABULARY',
    difficulty: 1
  },
  {
    id: 'q6',
    type: 'irregular',
    question: "'듣다' (nghe) kết hợp với '-어/아 보세요' sẽ biến đổi thành gì?",
    options: ['1) 듣어 보세요', '2) 들어 보세요', '3) 듬어 보세요', '4) 들으 보세요'],
    correct: 1,
    explanation: "'듣다' là bất quy tắc 'ㄷ' 불규칙. Khi gặp nguyên âm, 'ㄷ' chuyển thành 'ㄹ' -> '들어 보세요'.",
    topic: 'Grammar Irregular',
    skill: 'GRAMMAR',
    difficulty: 2
  },
  {
    id: 'q7',
    type: 'nuance_distinction',
    question: "Công ty cho nhân viên nghỉ phép thì dùng từ nào?",
    options: ['1) 방학', '2) 휴가', '3) 결석', '4) 소풍'],
    correct: 1,
    explanation: "Ở trường học dùng '방학' (kỳ nghỉ hè/đông), còn ở công ty dùng '휴가' (nghỉ phép). (Nguồn: Tip p.99)",
    topic: 'Vocabulary Nuance',
    skill: 'VOCABULARY',
    difficulty: 2
  }
];

export default function App() {
  // Navigation State
  const [currentTab, setCurrentTab] = useState('roadmap');

  // Plan & Goal Configuration
  const [targetExam, setTargetExam] = useState('Nov'); // 'Nov' or 'Jan'
  const [studyDays, setStudyDays] = useState(21);
  const [targetLevel, setTargetLevel] = useState('TOPIK II - 4급');
  const [dailyMinutes, setDailyMinutes] = useState(45);

  // User Performance State
  const [userXp, setUserXp] = useState(320);
  const [streakDays, setStreakDays] = useState(5);
  const [wrongBook, setWrongBook] = useState([]);
  const [historyLogs, setHistoryLogs] = useState([]);

  // Active Recall Card Index
  const [recallIndex, setRecallIndex] = useState(0);
  const [showRecallAnswer, setShowRecallAnswer] = useState(false);

  // Quiz State
  const [quizIndex, setQuizIndex] = useState(0);
  const [selectedOption, setSelectedOption] = useState(null);
  const [isAnswerSubmitted, setIsAnswerSubmitted] = useState(false);
  const [quizScore, setQuizScore] = useState(0);

  // Mock Exam State
  const [mockActive, setMockActive] = useState(false);
  const [mockTimeLeft, setMockTimeLeft] = useState(1200); // 20 mins
  const [mockAnswers, setMockAnswers] = useState({});
  const [mockSubmitted, setMockSubmitted] = useState(false);

  // Exam Day Checklist
  const [checklist, setChecklist] = useState({
    idCard: true,
    admissionTicket: true,
    markerPen: false,
    correctionTape: false,
    reviewWrongBook: true
  });

  // Calculate overall progress
  const totalVocabCount = SOURCE_VOCABULARY.length;
  const learnedCount = Math.min(18, totalVocabCount); // Simulated progress
  const progressPercent = Math.round((learnedCount / totalVocabCount) * 100);

  // Timer for Mock Exam
  useEffect(() => {
    let timer = null;
    if (mockActive && !mockSubmitted && mockTimeLeft > 0) {
      timer = setInterval(() => {
        setMockTimeLeft((prev) => prev - 1);
      }, 1000);
    }
    return () => clearInterval(timer);
  }, [mockActive, mockSubmitted, mockTimeLeft]);

  // Handler for answering Quiz Questions
  const handleAnswerSubmit = (optionIndex) => {
    if (isAnswerSubmitted) return;
    setSelectedOption(optionIndex);
    setIsAnswerSubmitted(true);

    const q = SOURCE_QUESTIONS[quizIndex];
    const isCorrect = optionIndex === q.correct;

    if (isCorrect) {
      setUserXp((prev) => prev + 20);
      setQuizScore((prev) => prev + 1);
    } else {
      // Add to Wrong Answer Book
      const exists = wrongBook.find((item) => item.id === q.id);
      if (!exists) {
        setWrongBook((prev) => [
          ...prev,
          {
            ...q,
            userWrongAnswer: optionIndex,
            failCount: 1,
            lastFailedAt: 'Vừa xong'
          }
        ]);
      } else {
        setWrongBook((prev) =>
          prev.map((item) =>
            item.id === q.id ? { ...item, failCount: item.failCount + 1 } : item
          )
        );
      }
    }

    setHistoryLogs((prev) => [
      ...prev,
      {
        questionId: q.id,
        isCorrect,
        timestamp: new Date().toLocaleTimeString()
      }
    ]);
  };

  const nextQuiz = () => {
    if (quizIndex < SOURCE_QUESTIONS.length - 1) {
      setQuizIndex((prev) => prev + 1);
      setSelectedOption(null);
      setIsAnswerSubmitted(false);
    } else {
      alert(`🎉 Bạn đã hoàn thành Quiz! Điểm số: ${quizScore}/${SOURCE_QUESTIONS.length}`);
    }
  };

  return (
    <div className="min-h-screen bg-amber-50/40 text-slate-800 font-sans pb-12">
      {/* HEADER NAVBAR */}
      <header className="sticky top-0 z-50 bg-white/90 backdrop-blur-md border-b border-amber-200/60 shadow-sm">
        <div className="max-w-5xl mx-auto px-4 py-3 flex items-center justify-between">
          <div className="flex items-center space-x-2">
            <span className="text-2xl">🇰🇷</span>
            <div>
              <h1 className="font-extrabold text-lg text-amber-900 leading-tight">TOPIK Master 2300</h1>
              <p className="text-xs text-amber-700">EdTech Gamified Platform</p>
            </div>
          </div>

          <div className="flex items-center space-x-3 text-sm">
            <div className="bg-amber-100 text-amber-800 px-3 py-1 rounded-full font-bold flex items-center gap-1">
              <span>🔥</span> <span>{streakDays} Ngày</span>
            </div>
            <div className="bg-emerald-100 text-emerald-800 px-3 py-1 rounded-full font-bold flex items-center gap-1">
              <span>⚡</span> <span>{userXp} XP</span>
            </div>
          </div>
        </div>

        {/* NAVIGATION TABS */}
        <div className="max-w-5xl mx-auto px-2 flex space-x-1 overflow-x-auto border-t border-slate-100 text-xs sm:text-sm font-medium">
          {[
            { id: 'roadmap', label: '🗺️ Roadmap', icon: '🗺️' },
            { id: 'plan', label: '🎯 Plan', icon: '🎯' },
            { id: 'learn', label: '📚 Learn', icon: '📚' },
            { id: 'recall', label: '🧠 Recall', icon: '🧠' },
            { id: 'quiz', label: '🎮 Quiz', icon: '🎮' },
            { id: 'wrong', label: `❌ Wrong (${wrongBook.length})`, icon: '❌' },
            { id: 'mock', label: '📝 Mock Test', icon: '📝' },
            { id: 'examday', label: '🚨 Exam Day', icon: '🚨' }
          ].map((tab) => (
            <button
              key={tab.id}
              onClick={() => setCurrentTab(tab.id)}
              className={`px-3 py-2.5 whitespace-nowrap transition-all border-b-2 ${
                currentTab === tab.id
                  ? 'border-amber-600 text-amber-900 font-bold bg-amber-100/50'
                  : 'border-transparent text-slate-500 hover:text-slate-800'
              }`}
            >
              {tab.label}
            </button>
          ))}
        </div>
      </header>

      {/* MAIN CONTAINER */}
      <main className="max-w-5xl mx-auto px-4 mt-6">
        {/* ========================================== */}
        {/* 1. ROADMAP VIEW */}
        {/* ========================================== */}
        {currentTab === 'roadmap' && (
          <div className="space-y-6">
            <div className="bg-gradient-to-r from-amber-500 to-orange-500 text-white p-6 rounded-2xl shadow-lg">
              <div className="flex justify-between items-start">
                <div>
                  <span className="bg-white/20 text-xs px-2.5 py-1 rounded-full font-semibold uppercase tracking-wider">
                    Giai đoạn 1 — TOPIK Tháng 11
                  </span>
                  <h2 className="text-2xl font-bold mt-2">TOPIK Master Roadmap</h2>
                  <p className="text-amber-100 text-sm mt-1">
                    Trích xuất trực tiếp từ giáo trình TOPIK VOCA 2300 chuẩn cấu trúc thi.
                  </p>
                </div>
                <div className="bg-white/20 p-3 rounded-xl text-center backdrop-blur-sm">
                  <div className="text-2xl font-black">{progressPercent}%</div>
                  <div className="text-[10px] uppercase font-bold text-amber-100">Hoàn thành</div>
                </div>
              </div>

              {/* Progress Bar */}
              <div className="w-full bg-black/20 rounded-full h-3 mt-4 overflow-hidden">
                <div
                  className="bg-white h-full transition-all duration-500 rounded-full"
                  style={{ width: `${progressPercent}%` }}
                />
              </div>
            </div>

            {/* Skill Modules Grid */}
            <h3 className="font-bold text-lg text-slate-800 flex items-center gap-2">
              <span>📚</span> Phân Vùng Kiến Thức (Source Modules)
            </h3>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {[
                {
                  title: '02 교육 - 교실 용어 (Education Terms)',
                  importance: '★★★★★',
                  items: '12 단어',
                  status: '▶️ In Progress',
                  accuracy: '88%',
                  color: 'border-amber-400 bg-amber-50/50'
                },
                {
                  title: '02 교육 - 수업 (Lessons & Classroom)',
                  importance: '★★★★★',
                  items: '15 단어 + Irregulars',
                  status: '▶️ In Progress',
                  accuracy: '75%',
                  color: 'border-amber-400 bg-amber-50/50'
                },
                {
                  title: '02 교육 - 시험 (Exams & TOPIK Format)',
                  importance: '★★★★★',
                  items: '16 단어 + Let\'s Check',
                  status: '🔥 Weak Area',
                  accuracy: '60%',
                  color: 'border-rose-400 bg-rose-50/40'
                },
                {
                  title: '02 교육 - 학교 (School Life)',
                  importance: '★★★★☆',
                  items: '10 단어',
                  status: '🔒 Locked',
                  accuracy: '0%',
                  color: 'border-slate-200 bg-slate-50'
                },
                {
                  title: 'Hanja Root: 學 (학) & 親 (친)',
                  importance: '★★★★☆',
                  items: 'Mindmap Voca',
                  status: '🔒 Locked',
                  accuracy: '0%',
                  color: 'border-slate-200 bg-slate-50'
                },
                {
                  title: 'Let\'s Check Real TOPIK Questions',
                  importance: '★★★★★',
                  items: '7 Questions',
                  status: '🏆 Mastered',
                  accuracy: '100%',
                  color: 'border-emerald-400 bg-emerald-50/40'
                }
              ].map((m, idx) => (
                <div key={idx} className={`p-4 rounded-xl border-2 ${m.color} shadow-sm space-y-2`}>
                  <div className="flex justify-between items-start">
                    <h4 className="font-bold text-slate-800 text-sm">{m.title}</h4>
                    <span className="text-xs font-bold px-2 py-0.5 rounded bg-white border shadow-xs">
                      {m.status}
                    </span>
                  </div>
                  <div className="flex justify-between items-center text-xs text-slate-600">
                    <span>Độ quan trọng: <strong className="text-amber-600">{m.importance}</strong></span>
                    <span>Số lượng: {m.items}</span>
                  </div>
                  <div className="text-xs text-slate-500 flex justify-between">
                    <span>Độ chính xác: <strong>{m.accuracy}</strong></span>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* 2. TARGET & STUDY PLAN VIEW */}
        {/* ========================================== */}
        {currentTab === 'plan' && (
          <div className="bg-white p-6 rounded-2xl border border-amber-200/80 shadow-sm space-y-6">
            <div>
              <h2 className="text-xl font-bold text-amber-900">🎯 Kế Hoạch & Mục Tiêu Luyện Thi</h2>
              <p className="text-sm text-slate-600">Tùy chỉnh thời gian học mà không bị cắt giảm kiến thức gốc.</p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              {/* Target Exam Selection */}
              <div className="space-y-2">
                <label className="text-xs font-bold text-slate-700 uppercase">Kỳ Thi Mục Tiêu</label>
                <div className="flex gap-2">
                  <button
                    onClick={() => setTargetExam('Nov')}
                    className={`flex-1 py-2.5 rounded-xl font-bold text-sm border-2 ${
                      targetExam === 'Nov'
                        ? 'border-amber-500 bg-amber-50 text-amber-900'
                        : 'border-slate-200 text-slate-600'
                    }`}
                  >
                    🍂 November TOPIK (Giai đoạn 1)
                  </button>
                  <button
                    onClick={() => setTargetExam('Jan')}
                    className={`flex-1 py-2.5 rounded-xl font-bold text-sm border-2 ${
                      targetExam === 'Jan'
                        ? 'border-amber-500 bg-amber-50 text-amber-900'
                        : 'border-slate-200 text-slate-600'
                    }`}
                  >
                    ❄️ January TOPIK (Giai đoạn 2)
                  </button>
                </div>
              </div>

              {/* Study Days Selection */}
              <div className="space-y-2">
                <label className="text-xs font-bold text-slate-700 uppercase">Thời Gian Ôn Tập</label>
                <div className="grid grid-cols-4 gap-2">
                  {[7, 14, 21, 30].map((days) => (
                    <button
                      key={days}
                      onClick={() => setStudyDays(days)}
                      className={`py-2 rounded-xl font-bold text-sm border-2 ${
                        studyDays === days
                          ? 'border-amber-500 bg-amber-50 text-amber-900'
                          : 'border-slate-200 text-slate-600'
                      }`}
                    >
                      {days} Ngày
                    </button>
                  ))}
                </div>
              </div>

              {/* Target Level */}
              <div className="space-y-2">
                <label className="text-xs font-bold text-slate-700 uppercase">Cấp Độ Mục Tiêu</label>
                <select
                  value={targetLevel}
                  onChange={(e) => setTargetLevel(e.target.value)}
                  className="w-full p-2.5 rounded-xl border-2 border-slate-200 font-semibold text-slate-800 text-sm bg-white"
                >
                  <option>TOPIK I - 1급</option>
                  <option>TOPIK I - 2급</option>
                  <option>TOPIK II - 3급</option>
                  <option>TOPIK II - 4급</option>
                  <option>TOPIK II - 5급</option>
                  <option>TOPIK II - 6급</option>
                </select>
              </div>

              {/* Daily Workload Calculation */}
              <div className="space-y-2">
                <label className="text-xs font-bold text-slate-700 uppercase">Thời Gian Học Mỗi Ngày</label>
                <div className="p-3 bg-amber-50 border border-amber-200 rounded-xl flex items-center justify-between">
                  <span className="text-sm font-medium text-amber-900">Dự kiến mỗi ngày:</span>
                  <span className="font-extrabold text-amber-700 text-base">Khoảng {Math.round(1000 / studyDays)} phút/ngày</span>
                </div>
              </div>
            </div>

            <div className="p-4 bg-slate-50 border border-slate-200 rounded-xl space-y-2">
              <h4 className="font-bold text-slate-800 text-sm">💡 Chiến Thuật Phân Bổ ({studyDays} Ngày)</h4>
              <p className="text-xs text-slate-600 leading-relaxed">
                {studyDays === 7 && 'Giai đoạn nước rút: Tập trung 80% Active Recall + Error Review & Mock Exam liên tục.'}
                {studyDays === 14 && 'Cân bằng học từ vựng mới mỗi ngày kết hợp 30 phút sửa sai và làm bài tập collocations.'}
                {studyDays === 21 && 'Phân bổ chuẩn: Học 2 Unit/ngày + Spaced Review định kỳ + 3 bài Mock Exam tổng hợp.'}
                {studyDays === 30 && 'Tối ưu nền tảng: Học sâu từ vựng gốc, Hanja mindmap, phân biệt từ nuốn và luyện viết.'}
              </p>
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* 3. LEARN MODE (SOURCE KNOWLEDGE) */}
        {/* ========================================== */}
        {currentTab === 'learn' && (
          <div className="space-y-6">
            <div className="flex justify-between items-center bg-white p-4 rounded-xl border border-amber-200">
              <div>
                <h2 className="text-lg font-bold text-amber-900">📚 Trích Xuất Dữ Liệu Giáo Trình</h2>
                <p className="text-xs text-slate-500">Giữ nguyên thuật ngữ tiếng Hàn + Giải thích Tiếng Việt</p>
              </div>
              <span className="text-xs bg-amber-100 text-amber-800 font-bold px-2.5 py-1 rounded-full">
                {SOURCE_VOCABULARY.length} Từ Vựng
              </span>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {SOURCE_VOCABULARY.map((v) => (
                <div key={v.id} className="bg-white p-4 rounded-2xl border border-slate-200 shadow-xs space-y-3">
                  <div className="flex justify-between items-start border-b border-slate-100 pb-2">
                    <div>
                      <div className="flex items-baseline space-x-2">
                        <span className="text-xl font-black text-slate-900">{v.korean}</span>
                        <span className="text-xs font-bold text-amber-700 bg-amber-100 px-1.5 py-0.5 rounded">
                          [{v.pos}]
                        </span>
                      </div>
                      <p className="text-xs text-slate-500 mt-0.5">{v.english}</p>
                    </div>
                    <span className="text-[10px] bg-slate-100 text-slate-600 font-medium px-2 py-0.5 rounded">
                      {v.unit}
                    </span>
                  </div>

                  {/* Vietnamese Meaning */}
                  <div className="text-sm font-semibold text-amber-950">
                    🇻🇳 Nghĩa: <span className="text-slate-800">{v.vietnamese}</span>
                  </div>

                  {/* Irregular or Special Forms */}
                  {v.irregular && (
                    <div className="text-xs bg-rose-50 text-rose-700 p-2 rounded-lg font-medium border border-rose-100">
                      ⚠️ Biến đổi bất quy tắc: <strong>{v.irregular}</strong>
                    </div>
                  )}

                  {/* Example Sentence */}
                  {v.example && (
                    <div className="bg-amber-50/60 p-2.5 rounded-xl border border-amber-100 text-xs space-y-1">
                      <div className="font-bold text-amber-900">📝 Ví dụ giáo trình:</div>
                      <div className="text-slate-800 font-medium">{v.example}</div>
                    </div>
                  )}

                  {/* Collocations & Tips */}
                  {v.collocation && (
                    <div className="text-xs text-emerald-800 bg-emerald-50 p-2 rounded-lg border border-emerald-100">
                      🔗 <strong>Collocation:</strong> {v.collocation}
                    </div>
                  )}

                  {v.tip && (
                    <div className="text-xs text-blue-800 bg-blue-50 p-2 rounded-lg border border-blue-100">
                      💡 <strong>Ghi chú bẫy TOPIK:</strong> {v.tip}
                    </div>
                  )}

                  {/* Synonyms / Antonyms */}
                  <div className="flex flex-wrap gap-2 text-[11px] pt-1">
                    {v.synonym && (
                      <span className="bg-slate-100 text-slate-700 px-2 py-0.5 rounded">
                        유 (Đồng nghĩa): <strong>{v.synonym}</strong>
                      </span>
                    )}
                    {v.antonym && (
                      <span className="bg-slate-100 text-slate-700 px-2 py-0.5 rounded">
                        반 (Trái nghĩa): <strong>{v.antonym}</strong>
                      </span>
                    )}
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* 4. ACTIVE RECALL MODE */}
        {/* ========================================== */}
        {currentTab === 'recall' && (
          <div className="max-w-xl mx-auto space-y-6">
            <div className="text-center space-y-1">
              <h2 className="text-xl font-bold text-amber-900">🧠 Active Recall Flashcard</h2>
              <p className="text-xs text-slate-600">Nhìn tiếng Hàn $\rightarrow$ Tự nhớ nghĩa & ví dụ $\rightarrow$ Kiểm tra đáp án</p>
            </div>

            {/* Flashcard Box */}
            <div className="bg-white rounded-3xl border-2 border-amber-300 p-8 shadow-md text-center space-y-6 min-h-[300px] flex flex-col justify-between">
              <div className="space-y-2">
                <span className="text-xs font-bold text-amber-600 bg-amber-100 px-3 py-1 rounded-full">
                  Card {recallIndex + 1} / {SOURCE_VOCABULARY.length}
                </span>
                <div className="text-4xl font-black text-slate-900 pt-4">
                  {SOURCE_VOCABULARY[recallIndex].korean}
                </div>
                <div className="text-xs text-slate-500 font-semibold">
                  [{SOURCE_VOCABULARY[recallIndex].pos}] • {SOURCE_VOCABULARY[recallIndex].unit}
                </div>
              </div>

              {/* Revealed Content */}
              {showRecallAnswer ? (
                <div className="p-4 bg-amber-50 rounded-2xl border border-amber-200 text-left space-y-2 animate-fadeIn">
                  <div className="text-base font-bold text-amber-900">
                    🇻🇳 Nghĩa: {SOURCE_VOCABULARY[recallIndex].vietnamese}
                  </div>
                  {SOURCE_VOCABULARY[recallIndex].example && (
                    <div className="text-xs text-slate-700">
                      <strong>Ví dụ:</strong> {SOURCE_VOCABULARY[recallIndex].example}
                    </div>
                  )}
                  {SOURCE_VOCABULARY[recallIndex].tip && (
                    <div className="text-xs text-blue-700">
                      <strong>Tip:</strong> {SOURCE_VOCABULARY[recallIndex].tip}
                    </div>
                  )}
                </div>
              ) : (
                <div className="text-sm text-slate-400 italic py-6">
                  👉 Hãy tự nhớ nghĩa trong đầu trước khi nhấn "Xem Đáp Án"
                </div>
              )}

              {/* Action Buttons */}
              <div className="space-y-3">
                <button
                  onClick={() => setShowRecallAnswer(!showRecallAnswer)}
                  className="w-full py-3 bg-amber-500 hover:bg-amber-600 text-white font-bold rounded-2xl shadow-sm transition"
                >
                  {showRecallAnswer ? '🙈 Hลง Đáp Án' : '👁️ Xem Đáp Án & Ví Dụ'}
                </button>

                <div className="flex gap-2">
                  <button
                    onClick={() => {
                      setShowRecallAnswer(false);
                      setRecallIndex((prev) => (prev > 0 ? prev - 1 : SOURCE_VOCABULARY.length - 1));
                    }}
                    className="flex-1 py-2 bg-slate-100 text-slate-700 font-bold rounded-xl text-xs"
                  >
                    ⬅️ Từ Trước
                  </button>
                  <button
                    onClick={() => {
                      setShowRecallAnswer(false);
                      setRecallIndex((prev) => (prev < SOURCE_VOCABULARY.length - 1 ? prev + 1 : 0));
                    }}
                    className="flex-1 py-2 bg-slate-100 text-slate-700 font-bold rounded-xl text-xs"
                  >
                    Từ Tiếp theo ➡️
                  </button>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* 5. GAMIFIED QUIZ SYSTEM */}
        {/* ========================================== */}
        {currentTab === 'quiz' && (
          <div className="max-w-2xl mx-auto space-y-6">
            {/* Quiz Header */}
            <div className="bg-white p-4 rounded-xl border border-amber-200 flex justify-between items-center">
              <div>
                <span className="text-xs font-bold text-amber-700 uppercase">Luyện Tập Dạng Câu Hỏi TOPIK</span>
                <h3 className="text-base font-bold text-slate-800">
                  Câu hỏi {quizIndex + 1} / {SOURCE_QUESTIONS.length}
                </h3>
              </div>
              <div className="text-right">
                <span className="text-xs text-slate-500">Điểm hiện tại</span>
                <div className="text-lg font-black text-amber-600">{quizScore} / {SOURCE_QUESTIONS.length}</div>
              </div>
            </div>

            {/* Question Card */}
            <div className="bg-white p-6 rounded-2xl border-2 border-amber-200 shadow-sm space-y-4">
              {SOURCE_QUESTIONS[quizIndex].passage && (
                <div className="p-3 bg-slate-50 border border-slate-200 rounded-xl text-xs font-mono whitespace-pre-line text-slate-800">
                  {SOURCE_QUESTIONS[quizIndex].passage}
                </div>
              )}

              <h4 className="font-bold text-base text-slate-900">
                {SOURCE_QUESTIONS[quizIndex].question}
              </h4>

              {/* Multiple Choice Options */}
              <div className="space-y-2 pt-2">
                {SOURCE_QUESTIONS[quizIndex].options.map((opt, optIdx) => {
                  let btnStyle = 'border-slate-200 hover:border-amber-400 bg-white text-slate-800';

                  if (isAnswerSubmitted) {
                    if (optIdx === SOURCE_QUESTIONS[quizIndex].correct) {
                      btnStyle = 'border-emerald-500 bg-emerald-50 text-emerald-900 font-bold';
                    } else if (selectedOption === optIdx) {
                      btnStyle = 'border-rose-500 bg-rose-50 text-rose-900 font-bold';
                    }
                  }

                  return (
                    <button
                      key={optIdx}
                      onClick={() => handleAnswerSubmit(optIdx)}
                      disabled={isAnswerSubmitted}
                      className={`w-full text-left p-3.5 rounded-xl border-2 transition text-sm flex justify-between items-center ${btnStyle}`}
                    >
                      <span>{opt}</span>
                      {isAnswerSubmitted && optIdx === SOURCE_QUESTIONS[quizIndex].correct && (
                        <span className="text-emerald-600 font-bold">✓ Đúng</span>
                      )}
                      {isAnswerSubmitted && selectedOption === optIdx && optIdx !== SOURCE_QUESTIONS[quizIndex].correct && (
                        <span className="text-rose-600 font-bold">✗ Sai</span>
                      )}
                    </button>
                  );
                })}
              </div>

              {/* Detailed Explanation */}
              {isAnswerSubmitted && (
                <div className="p-4 bg-amber-50 border border-amber-200 rounded-xl space-y-1 text-xs text-amber-950 animate-fadeIn">
                  <div className="font-bold text-amber-900">💡 Giải thích chi tiết từ giáo trình:</div>
                  <p>{SOURCE_QUESTIONS[quizIndex].explanation}</p>
                </div>
              )}

              {/* Next Question Button */}
              {isAnswerSubmitted && (
                <button
                  onClick={nextQuiz}
                  className="w-full py-3 bg-amber-500 hover:bg-amber-600 text-white font-bold rounded-xl text-sm shadow-sm"
                >
                  {quizIndex < SOURCE_QUESTIONS.length - 1 ? 'Câu Tiếp Theo ➡️' : 'Hoàn Thành Quiz 🏆'}
                </button>
              )}
            </div>
          </div>
        )}

        {/* ========================================== */}
        {/* 6. WRONG ANSWER BOOK (ERROR BOOK) */}
        {/* ========================================== */}
        {currentTab === 'wrong' && (
          <div className="space-y-6">
            <div className="bg-white p-4 rounded-xl border border-rose-200 flex justify-between items-center">
              <div>
                <h2 className="text-lg font-bold text-rose-900">❌ Wrongs & Error Book</h2>
                <p className="text-xs text-slate-500">
                  Danh sách câu trả lời sai tự động lưu để ôn tập lại theo cơ chế Spaced Repetition.
                </p>
              </div>
              <span className="bg-rose-100 text-rose-800 font-bold px-3 py-1 rounded-full text-xs">
                {wrongBook.length} Câu Cần Sửa
              </span>
            </div>

            {wrongBook.length === 0 ? (
              <div className="bg-white p-12 text-center rounded-2xl border border-slate-200 space-y-2">
                <span className="text-4xl">🎉</span>
                <h3 className="font-bold text-slate-700">Chưa có câu sai nào!</h3>
                <p className="text-xs text-slate-500">Hãy qua tab Quiz hoặc Mock Test để làm bài tập.</p>
              </div>
            ) : (
              <div className="space-y-4">
                {wrongBook.map((item, idx) => (
                  <div key={idx} className="bg-white p-5 rounded-2xl border-2 border-rose-200 shadow-xs space-y-3">
                    <div className="flex justify-between text-xs">
                      <span className="bg-rose-100 text-rose-800 font-bold px-2 py-0.5 rounded">
                        Số lần sai: {item.failCount}
                      </span>
                      <span className="text-slate-500">Kỹ năng: {item.skill} • {item.topic}</span>
                    </div>

                    <h4 className="font-bold text-slate-900 text-sm">{item.question}</h4>

                    <div className="bg-rose-50 p-2.5 rounded-xl border border-rose-100 text-xs text-rose-900">
                      <strong>Đáp án đúng:</strong> {item.options[item.correct]}
                    </div>

                    <div className="bg-amber-50 p-2.5 rounded-xl border border-amber-100 text-xs text-amber-900">
                      <strong>Giải thích:</strong> {item.explanation}
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>
        )}

        {/* ========================================== */}
        {/* 7. MOCK EXAM SIMULATION */}
        {/* ========================================== */}
        {currentTab === 'mock' && (
          <div className="space-y-6">
            {!mockActive ? (
              <div className="bg-white p-8 rounded-2xl border-2 border-amber-300 text-center space-y-4 shadow-sm">
                <span className="text-4xl">📝</span>
                <h2 className="text-2xl font-black text-amber-900">TOPIK Mock Exam Simulation</h2>
                <p className="text-xs text-slate-600 max-w-md mx-auto">
                  Mô phỏng áp lực thời gian làm bài thực tế. Đáp án và điểm số chi tiết chỉ được hiển thị sau khi Nộp Bài (Submit).
                </p>

                <div className="p-4 bg-amber-50 rounded-xl max-w-xs mx-auto text-xs text-amber-900 space-y-1">
                  <div>⏱️ <strong>Thời gian:</strong> 20 phút</div>
                  <div>📊 <strong>Số câu hỏi:</strong> {SOURCE_QUESTIONS.length} câu</div>
                  <div>🚫 Không xem đáp án trong lúc làm</div>
                </div>

                <button
                  onClick={() => {
                    setMockActive(true);
                    setMockSubmitted(false);
                    setMockAnswers({});
                    setMockTimeLeft(1200);
                  }}
                  className="px-8 py-3 bg-amber-500 hover:bg-amber-600 text-white font-extrabold rounded-2xl shadow-md text-sm"
                >
                  🚀 Bắt Đầu Làm Đề Thi Thử
                </button>
              </div>
            ) : (
              <div className="space-y-6">
                {/* Timer Bar */}
                <div className="bg-slate-900 text-white p-4 rounded-xl flex justify-between items-center sticky top-16 z-40">
                  <span className="font-bold text-sm">📝 TOPIK Mock Exam in Progress</span>
                  <div className="bg-amber-500 text-white font-mono font-bold px-3 py-1 rounded-lg text-sm">
                    ⏱️ {Math.floor(mockTimeLeft / 60)}:{(mockTimeLeft % 60).toString().padStart(2, '0')}
                  </div>
                </div>

                {/* Questions List */}
                <div className="space-y-6">
                  {SOURCE_QUESTIONS.map((q, idx) => (
                    <div key={q.id} className="bg-white p-6 rounded-2xl border border-slate-200 shadow-xs space-y-3">
                      <div className="text-xs font-bold text-amber-700">Câu {idx + 1}</div>
                      {q.passage && (
                        <div className="p-3 bg-slate-50 border border-slate-200 rounded-xl text-xs font-mono">
                          {q.passage}
                        </div>
                      )}
                      <h4 className="font-bold text-slate-900 text-sm">{q.question}</h4>

                      <div className="space-y-2 pt-1">
                        {q.options.map((opt, optIdx) => (
                          <label
                            key={optIdx}
                            className={`flex items-center space-x-3 p-3 rounded-xl border-2 cursor-pointer transition text-xs ${
                              mockAnswers[q.id] === optIdx
                                ? 'border-amber-500 bg-amber-50 font-bold text-amber-900'
                                : 'border-slate-200 bg-white text-slate-700'
                            }`}
                          >
                            <input
                              type="radio"
                              name={`mock_${q.id}`}
                              checked={mockAnswers[q.id] === optIdx}
                              onChange={() => setMockAnswers({ ...mockAnswers, [q.id]: optIdx })}
                              disabled={mockSubmitted}
                            />
                            <span>{opt}</span>
                          </label>
                        ))}
                      </div>

                      {mockSubmitted && (
                        <div className="p-3 bg-amber-50 border border-amber-200 rounded-xl text-xs text-amber-950 space-y-1">
                          <div className="font-bold">
                            {mockAnswers[q.id] === q.correct ? '✅ Đúng' : '❌ Sai'} — Đáp án đúng: {q.options[q.correct]}
                          </div>
                          <div>{q.explanation}</div>
                        </div>
                      )}
                    </div>
                  ))}
                </div>

                {/* Submit Button */}
                {!mockSubmitted ? (
                  <button
                    onClick={() => setMockSubmitted(true)}
                    className="w-full py-4 bg-emerald-600 hover:bg-emerald-700 text-white font-black rounded-2xl shadow-lg text-base"
                  >
                    📥 Nộp Bài Thi Thử & Xem Điểm Số
                  </button>
                ) : (
                  <button
                    onClick={() => setMockActive(false)}
                    className="w-full py-4 bg-slate-800 text-white font-black rounded-2xl shadow-lg text-base"
                  >
                    🔄 Quay Lai Màn Hình Chuẩn Bị
                  </button>
                )}
              </div>
            )}
          </div>
        )}

        {/* ========================================== */}
        {/* 8. EXAM DAY MODE */}
        {/* ========================================== */}
        {currentTab === 'examday' && (
          <div className="space-y-6">
            <div className="bg-rose-600 text-white p-6 rounded-2xl shadow-md space-y-2">
              <span className="text-xs bg-white/20 font-bold px-2.5 py-1 rounded-full uppercase">
                🚨 EXAM DAY CHECKLIST
              </span>
              <h2 className="text-2xl font-black">Chuẩn Bị Cho Ngày Thi TOPIK</h2>
              <p className="text-xs text-rose-100">
                Danh sách những vật dụng và công việc bắt buộc phải kiểm tra trước khi đến phòng thi.
              </p>
            </div>

            <div className="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm space-y-4">
              <h3 className="font-bold text-slate-800 text-base">☑️ Checklist Phòng Thi</h3>

              <div className="space-y-3">
                {[
                  { key: 'idCard', label: 'Chứng minh thư / Căn cước công dân / Hộ chiếu gốc' },
                  { key: 'admissionTicket', label: 'Phiếu báo danh (수험표) đã in sẵn' },
                  { key: 'markerPen', label: 'Bút viết bảng / Bút chì dự phòng' },
                  { key: 'correctionTape', label: 'Băng xóa kéo (수정테이프)' },
                  { key: 'reviewWrongBook', label: 'Ôn nhanh lại Error Book & Bẫy 다르다/틀리다' }
                ].map((item) => (
                  <label
                    key={item.key}
                    onClick={() => setChecklist({ ...checklist, [item.key]: !checklist[item.key] })}
                    className={`flex items-center space-x-3 p-3.5 rounded-xl border-2 cursor-pointer transition text-sm ${
                      checklist[item.key]
                        ? 'border-emerald-500 bg-emerald-50/50 text-emerald-950 font-semibold'
                        : 'border-slate-200 bg-white text-slate-600'
                    }`}
                  >
                    <input
                      type="checkbox"
                      checked={checklist[item.key]}
                      readOnly
                      className="w-4 h-4 text-emerald-600 rounded"
                    />
                    <span>{item.label}</span>
                  </label>
                ))}
              </div>
            </div>

            <div className="bg-amber-50 border border-amber-200 p-5 rounded-2xl space-y-2">
              <h4 className="font-bold text-amber-900 text-sm">💡 Lời Khuyên Giờ G Chặn Lỗi</h4>
              <ul className="text-xs text-amber-950 space-y-1.5 list-disc pl-4">
                <li>Đọc kỹ yêu cầu câu hỏi: chọn đáp án <strong>알맞은 것</strong> (cái đúng) hay <strong>알맞지 않은 것</strong> (cái không đúng).</li>
                <li>Không bỏ trống đáp án TOPIK 읽기 (kể cả khi đoán).</li>
                <li>Gạch chân dưới các từ khóa quan trọng ngay khi đọc câu hỏi (`밑줄을 치다`).</li>
              </ul>
            </div>
          </div>
        )}
      </main>
    </div>
  );
}