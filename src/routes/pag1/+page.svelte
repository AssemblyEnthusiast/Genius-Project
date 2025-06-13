<div class="w-full bg-red-800">Events</div>
<div class="w-full flex items-center justify-center">
    <iframe src="https://calendar.google.com/calendar/embed?height=600&wkst=1&ctz=America%2FNew_York&showPrint=0&src=MjdmcmlzYmVlakB3aWx0b25wcy5vcmc&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTIyZDg5OWVkQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTdmM2VhNmEzQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbWM1ZDdhMGMxQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTRhNWIzYTBhQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTBkZDkyMGQ0QGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTU0ZmI0MThkQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTA5YzA2ZjNlQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=ZW4udXNhI2hvbGlkYXlAZ3JvdXAudi5jYWxlbmRhci5nb29nbGUuY29t&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTM1MWVhYTIxQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbWU3OTdiOWJiQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbWFiOThjMDE2QGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTk2OWI1MTE0QGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTAwNzhjNTNkQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTBhZWE3YjRhQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTM1N2NlNDhiQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTE2OGZlMmFlQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&src=d2lsdG9ucHMub3JnX2NsYXNzcm9vbTBlYjYzY2U0QGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&color=%23039BE5&color=%233e2723&color=%23202124&color=%23880e4f&color=%234a148c&color=%230047a8&color=%230047a8&color=%233e2723&color=%230B8043&color=%234a148c&color=%23004d40&color=%23137333&color=%23174ea6&color=%233e2723&color=%233e2723&color=%231c3aa9&color=%23137333&color=%2300457c" style="border:solid 1px #777" width="800" height="600" frameborder="0" scrolling="no"></iframe>
</div>

<script lang="ts">
  // You need to set up OAuth2 and Google API client
  // This is a simplified example
  let eventTitle = $state('');
  let eventDate = $state('');

  async function addEvent() {
    // Load Google API client if not already loaded
    if (!window.gapi) {
      alert("Google API client not loaded.");
      return;
    }

    // Initialize client if not already initialized
    if (!window.gapi.client?.calendar) {
      await window.gapi.client.init({
      apiKey: "AIzaSyCY6gLJlXuW0x7-0S8rsqy2vum6M29Z_uM",
      discoveryDocs: ["https://www.googleapis.com/discovery/v1/apis/calendar/v3/rest"],
      });
    }

    // Prompt user to sign in (public API key only allows read, for write you need OAuth2)
    // This will only work for public calendars with insert permission enabled
    // For production, use OAuth2 flow
    try {
      const response = await window.gapi.client.calendar.events.insert({
      calendarId: "primary",
      resource: {
        summary: eventTitle,
        start: { date: eventDate },
        end: { date: eventDate }
      }
      });
      alert("Event added: " + response.result.summary);
    } catch (err) {
      alert("Failed to add event: " + err.message);
    }
    // See: https://developers.google.com/calendar/api/quickstart/js
  }
</script>

<input bind:value={eventTitle} placeholder="Event Title" />
<input type="date" bind:value={eventDate} />
<button class="p-4 bg-white" onclick={() => {alert("Added event " + eventTitle + " on " + eventDate)}}>Add Event</button>
