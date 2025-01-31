import React, { useEffect, useState } from "react";

const CalendarPage = () => {
  return (
    <div>
      <h1>Connect Your Calendar</h1>
      <a href="https://accounts.google.com/o/oauth2/auth?client_id=YOUR_CLIENT_ID&redirect_uri=YOUR_REDIRECT_URI&response_type=code&scope=https://www.googleapis.com/auth/calendar.events">
        Connect Google Calendar
      </a>
    </div>
  );
};

export default CalendarPage;
const express = require("express");
const { google } = require("googleapis");

const app = express();
const calendar = google.calendar("v3");

app.get("/events", async (req, res) => {
  const auth = new google.auth.OAuth2(YOUR_CLIENT_ID, YOUR_CLIENT_SECRET, YOUR_REDIRECT_URI);
  auth.setCredentials({ access_token: req.query.token });

  const response = await calendar.events.list({
    auth,
    calendarId: "primary",
    timeMin: new Date().toISOString(),
    maxResults: 10,
    singleEvents: true,
    orderBy: "startTime",
  });

  res.json(response.data.items);
});

app.listen(3000, () => console.log("Server started on port 3000"));
import React, { useEffect, useState } from "react";
import FullCalendar from "@fullcalendar/react";
import dayGridPlugin from "@fullcalendar/daygrid";

const CalendarComponent = () => {
  const [events, setEvents] = useState([]);

  useEffect(() => {
    fetch("/events?token=YOUR_ACCESS_TOKEN")
      .then((response) => response.json())
      .then((data) =>
        setEvents(data.map((event) => ({ title: event.summary, date: event.start.dateTime })))
      );
  }, []);

  return <FullCalendar plugins={[dayGridPlugin]} events={events} />;
};

export default CalendarComponent;
