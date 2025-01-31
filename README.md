
// Next.js frontend (pages/index.js) - Homepage with Calendar, Alarm, Surveys, Checklists
import { useState, useEffect } from "react";
import { useRouter } from "next/router";

export default function Home() {
  const [alarmTime, setAlarmTime] = useState("");
  const [message, setMessage] = useState("");
  const [answers, setAnswers] = useState({});
  const [interviewAnswers, setInterviewAnswers] = useState({});

  const questions = [
    {
      category: "Effectiveness",
      items: [
        "Does your focus got improved after using AOGS website for days?",
        "Does the AOGS influenced you to track down your activities every day?",
        "Does AOGS website prevents you from playing online games excessively?",
        "Would tracking both your gaming and academic time in one place motivate you to stick to a balanced schedule?"
      ]
    },
    {
      category: "Academic Engagements",
      items: [
        "Do you still procrastinate your schoolwork’s?",
        "Do you think that the website helps you identify how enough is your time to manage school and everyday needs?",
        "Does the website give urge and motivation to do academic activities?"
      ]
    },
    {
      category: "Effects (Expected Outcome)",
      items: [
        "Does the website help you organize everything in time?",
        "Do you think you could be able to apply AOGS and for it to be part of your everyday life?",
        "Do you think having a structured schedule, based on your gaming and study habits, will make you more efficient with your time?",
        "Do you believe the use of AOGS will have a positive long-term effect on your ability to balance school and personal interests like gaming?",
        "Do you think the platform will help you develop better habits for managing your time and tasks in the future?"
      ]
    }
  ];

  const interviewQuestions = [
    "Does AOGS help you ease the usage of online gaming?",
    "What are the visible effects of using the website?",
    "How does AOGS improve your focus to be engaged in academic activities?",
    "What are the effects of AOGS usage on your mental state?",
    "How do you feel about using a platform that tracks both your academic progress and online gaming time? Would you find that helpful?"
  ];

  const handleAnswerChange = (question, answer) => {
    setAnswers((prev) => ({ ...prev, [question]: answer }));
  };

  const handleInterviewAnswerChange = (question, answer) => {
    setInterviewAnswers((prev) => ({ ...prev, [question]: answer }));
  };

  const setAlarm = async () => {
    if (!alarmTime) return alert("Please set a time.");
    const res = await fetch("/api/alarm", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ time: alarmTime }),
    });
    const data = await res.json();
    setMessage(data.message);
  };

  return (
    <div className="container">
      <h1>Calendar & Alarm App</h1>
      <input type="time" value={alarmTime} onChange={(e) => setAlarmTime(e.target.value)} />
      <button onClick={setAlarm}>Set Alarm</button>
      {message && <p>{message}</p>}
      
      <h2>Checklist Questionnaire</h2>
      {questions.map((section) => (
        <div key={section.category}>
          <h3>{section.category}</h3>
          <table border="1">
            <thead>
              <tr>
                <th>Question</th>
                <th>Yes</th>
                <th>No</th>
              </tr>
            </thead>
            <tbody>
              {section.items.map((question) => (
                <tr key={question}>
                  <td>{question}</td>
                  <td>
                    <input type="radio" name={question} value="Yes" onChange={() => handleAnswerChange(question, "Yes")} />
                  </td>
                  <td>
                    <input type="radio" name={question} value="No" onChange={() => handleAnswerChange(question, "No")} />
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      ))}
      
      <h2>Interview Questionnaire</h2>
      <table border="1">
        <thead>
          <tr>
            <th>Question</th>
            <th>Answer</th>
          </tr>
        </thead>
        <tbody>
          {interviewQuestions.map((question) => (
            <tr key={question}>
              <td>{question}</td>
              <td>
                <textarea rows="2" cols="50" onChange={(e) => handleInterviewAnswerChange(question, e.target.value)}></textarea>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
