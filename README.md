// pages/Result.jsx
import { useEffect, useState } from 'react';
import { CheckCircle } from 'lucide-react';

export default function Result() {
  const [summary, setSummary] = useState('');
  const [score, setScore] = useState(0);
  const [tagline, setTagline] = useState('');
  const [careers, setCareers] = useState([]);
  const [personality, setPersonality] = useState('');

  useEffect(() => {
    const storedAnswers = JSON.parse(localStorage.getItem('careerAnswers'));
    if (!storedAnswers) return;

    const calcScore = storedAnswers.reduce((acc, ans) => {
      switch (ans) {
        case 'Stay calm':
        case 'Always':
        case 'Logically':
        case 'Very':
        case 'Learn and move on':
          return acc + 3;
        case 'Try again':
        case 'Sometimes':
        case 'By asking others':
        case 'Average':
          return acc + 2;
        case 'Get angry':
        case 'Rarely':
        case 'Emotionally':
        case 'Not much':
        case 'Get demotivated':
          return acc + 1;
        default:
          return acc;
      }
    }, 0);

    setScore(calcScore);

    if (calcScore >= 13) {
      setSummary('You have a strong and resilient personality.');
      setTagline('Born Leader');
      setCareers(['Entrepreneurship', 'Strategic Management', 'Leadership Roles']);
      setPersonality('Elon Musk');
    } else if (calcScore >= 9) {
      setSummary('You have a balanced personality.');
      setTagline('The Analyzer');
      setCareers(['Consulting', 'Customer Relations', 'Business Analysis']);
      setPersonality('Barack Obama');
    } else {
      setSummary('You are supportive and creative.');
      setTagline('The Creator');
      setCareers(['Design', 'Arts', 'Creative Assistant Roles']);
      setPersonality('Steve Jobs');
    }
  }, []);

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-r from-green-100 to-blue-100 p-6">
      <div className="w-full max-w-3xl bg-white p-8 rounded-2xl shadow-xl">
        <h2 className="text-3xl font-bold text-center text-green-700 mb-2">Your Personality Insight</h2>
        <p className="text-center text-lg text-gray-600 mb-6">{summary}</p>

        <div className="flex flex-col md:flex-row gap-6">
          {/* Score Card */}
          <div className="flex-1 bg-green-50 p-6 rounded-xl shadow-md">
            <h3 className="text-xl font-semibold text-green-800 mb-2">Your Score</h3>
            <p className="text-5xl font-bold text-green-600">{score}/15</p>
          </div>

          {/* Tagline & Careers */}
          <div className="flex-1 bg-blue-50 p-6 rounded-xl shadow-md">
            <h3 className="text-xl font-semibold text-blue-800 mb-2">You are: <span className="italic font-bold">{tagline}</span></h3>
            <ul className="list-none mt-4 space-y-2">
              {careers.map((career, idx) => (
                <li key={idx} className="flex items-center text-blue-700">
                  <CheckCircle className="h-5 w-5 mr-2 text-blue-500" /> {career}
                </li>
              ))}
            </ul>
          </div>
        </div>

        {/* Famous Personality */}
        <div className="mt-8 bg-yellow-50 p-6 rounded-xl shadow-md text-center">
          <h3 className="text-xl font-semibold text-yellow-800 mb-2">You resemble:</h3>
          <p className="text-2xl font-bold text-yellow-600">{personality}</p>
        </div>


        {/* Note for school students */}
        <div className="mt-6 text-center text-sm text-gray-600">
          <p>*Career suggestions are most suitable for students up to 12th grade.*</p>
        </div>

        <div className="mt-8 text-center">
          <p className="text-gray-500 text-sm">This result is a basic insight. For detailed guidance, consult a career counselor.</p>
        </div>
      </div>
    </div>
  );
}
