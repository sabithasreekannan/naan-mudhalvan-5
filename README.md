<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Email Reminder System</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f6fa;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 600px;
      margin: auto;
      background: #fff;
      padding: 25px;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
    }

    h1 {
      text-align: center;
      color: #2f3640;
    }

    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
    }

    input, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 5px;
      box-sizing: border-box;
    }

    button {
      margin-top: 15px;
      padding: 10px 15px;
      background: #0984e3;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background: #74b9ff;
    }

    .reminder {
      border-bottom: 1px solid #ddd;
      padding: 10px 0;
    }

    .reminder p {
      margin: 5px 0;
    }

    .delete-btn {
      background: #d63031;
      color: #fff;
      border: none;
      padding: 5px 10px;
      border-radius: 3px;
      cursor: pointer;
    }

    .delete-btn:hover {
      background: #ff7675;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Email Reminder System</h1>

    <form id="reminderForm">
      <label for="email">Recipient Email:</label>
      <input type="email" id="email" required placeholder="example@gmail.com">

      <label for="subject">Subject:</label>
      <input type="text" id="subject" required placeholder="Meeting Reminder">

      <label for="message">Message:</label>
      <textarea id="message" rows="4" required placeholder="Don't forget the meeting at 5 PM!"></textarea>

      <label for="datetime">Reminder Date & Time:</label>
      <input type="datetime-local" id="datetime" required>

      <button type="submit">Set Reminder</button>
    </form>

    <h2>Scheduled Reminders</h2>
    <div id="reminderList"></div>
  </div>

  <audio id="alertSound" src="https://www.soundjay.com/button/sounds/beep-07.mp3" preload="auto"></audio>

  <script>
    const reminderForm = document.getElementById('reminderForm');
    const reminderList = document.getElementById('reminderList');
    const alertSound = document.getElementById('alertSound');

    // Ask for Notification permission
    if (Notification.permission !== "granted") {
      Notification.requestPermission();
    }

    // Load reminders from localStorage
    let reminders = JSON.parse(localStorage.getItem('reminders')) || [];

    // Display reminders
    function displayReminders() {
      reminderList.innerHTML = '';
      if (reminders.length === 0) {
        reminderList.innerHTML = '<p>No reminders yet.</p>';
        return;
      }

      reminders.forEach((reminder, index) => {
        const div = document.createElement('div');
        div.className = 'reminder';
        div.innerHTML = `
          <p><strong>To:</strong> ${reminder.email}</p>
          <p><strong>Subject:</strong> ${reminder.subject}</p>
          <p><strong>Message:</strong> ${reminder.message}</p>
          <p><strong>Time:</strong> ${new Date(reminder.datetime).toLocaleString()}</p>
          <button class="delete-btn" onclick="deleteReminder(${index})">Delete</button>
        `;
        reminderList.appendChild(div);
      });
    }

    // Add reminder
    reminderForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const email = document.getElementById('email').value;
      const subject = document.getElementById('subject').value;
      const message = document.getElementById('message').…
