<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>A Quick Question</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: #ffeef2;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      text-align: center;
      padding: 20px;
    }

    .card {
      background: white;
      padding: 35px 25px;
      border-radius: 24px;
      box-shadow: 0 10px 30px rgba(255, 105, 180, 0.2);
      max-width: 450px;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
    }

    .sticker {
      width: 160px;
      height: 160px;
      object-fit: contain;
    }

    h1 {
      color: #d81b60;
      font-size: 1.6rem;
      line-height: 1.3;
    }

    .btn-container {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 16px;
      flex-wrap: wrap;
      width: 100%;
      margin-top: 10px;
    }

    button {
      border: none;
      cursor: pointer;
      font-weight: 700;
      border-radius: 9999px;
      transition: transform 0.2s ease, font-size 0.2s ease, padding 0.2s ease;
      font-family: inherit;
    }

    button:active {
      transform: scale(0.96);
    }

    #yesBtn {
      background-color: #22c55e;
      color: white;
      font-size: 1.1rem;
      padding: 12px 28px;
    }

    #noBtn {
      background-color: #ef4444;
      color: white;
      font-size: 1.1rem;
      padding: 12px 24px;
    }

    /* Date and Time Picker Form */
    #dateFormContainer {
      display: none;
      width: 100%;
      flex-direction: column;
      gap: 12px;
      align-items: center;
      animation: fadeIn 0.5s ease-in-out;
    }

    .input-field {
      padding: 12px 16px;
      border-radius: 12px;
      border: 2px solid #ffccd5;
      outline: none;
      font-size: 1rem;
      font-family: inherit;
      color: #333;
      width: 85%;
      background: #fff;
    }

    .input-field:focus {
      border-color: #d81b60;
    }

    .input-label {
      font-size: 0.85rem;
      color: #777;
      margin-bottom: -6px;
      align-self: flex-start;
      margin-left: 8%;
    }

    .submit-btn {
      background-color: #d81b60;
      color: white;
      font-size: 1.1rem;
      padding: 12px 28px;
      margin-top: 8px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>

  <div class="card" id="questionCard">
    <img 
      id="mainImage" 
      class="sticker" 
      src="https://media1.tenor.com/m/hGe0J89tuW0AAAAC/nod-cat-hyper.gif" 
      alt="Nodding Cat"
    />
    <h1 id="questionText">Will you go out with me?</h1>
    
    <div class="btn-container" id="initialButtons">
      <button id="yesBtn">Yes</button>
      <button id="noBtn">No</button>
    </div>

    <div id="dateFormContainer">
      <p style="color: #666; font-size: 0.95rem; font-weight: 600;">Pick our date & time! 🥰</p>
      
      <span class="input-label">Date:</span>
      <input type="date" id="selectedDate" class="input-field" required />
      
      <span class="input-label">Time:</span>
      <input type="time" id="selectedTime" class="input-field" required />
      
      <button id="submitDateBtn" class="submit-btn">Lock it in! 💌</button>
    </div>
  </div>

  <script>
    const yesBtn = document.getElementById("yesBtn");
    const noBtn = document.getElementById("noBtn");
    const mainImage = document.getElementById("mainImage");
    const questionText = document.getElementById("questionText");
    const initialButtons = document.getElementById("initialButtons");
    const dateFormContainer = document.getElementById("dateFormContainer");
    const submitDateBtn = document.getElementById("submitDateBtn");
    const selectedDateInput = document.getElementById("selectedDate");
    const selectedTimeInput = document.getElementById("selectedTime");

    // Restrict calendar so past dates cannot be picked
    const today = new Date().toISOString().split("T")[0];
    selectedDateInput.setAttribute("min", today);

    const noPhrases = [
      "No",
      "Ihh sige na",
      "plss :((",
      "Libre ko samg",
      "Tapos nood cine",
      "Yiee papayag na yan",
      "Sige ka masasad ako",
      "Sige na plss",
      "Kiss kita marami",
      "Ihh noo :(",
      "Kawawa naman ako!",
      "Masasadd na ako",
      "Plsss",
      "Kakain tayo ket saan mo want"
    ];

    let noClickCount = 0;
    let yesFontSize = 1.1;
    let yesPaddingX = 28;
    let yesPaddingY = 12;

    noBtn.addEventListener("click", () => {
      noClickCount++;

      yesFontSize += 0.45;
      yesPaddingX += 10;
      yesPaddingY += 6;

      yesBtn.style.fontSize = `${yesFontSize}rem`;
      yesBtn.style.padding = `${yesPaddingY}px ${yesPaddingX}px`;

      const phraseIndex = Math.min(noClickCount, noPhrases.length - 1);
      noBtn.innerText = noPhrases[phraseIndex];

      if (noClickCount === 2) {
        mainImage.src = "https://media.giphy.com/media/OPU6wzx8JrHna/giphy.gif";
      }
    });

    yesBtn.addEventListener("click", () => {
      questionText.innerText = "YAY!! Libre ko, anw when are you free? ❤️";
      mainImage.src = "https://media.giphy.com/media/T86i6yDyOYz7J6dPhf/giphy.gif";
      
      initialButtons.style.display = "none";
      dateFormContainer.style.display = "flex";
    });

    // Helper function to turn 24h format (e.g. 17:30) into 12h format (5:30 PM)
    function formatTime(timeStr) {
      const [hour, minute] = timeStr.split(":");
      let h = parseInt(hour, 10);
      const ampm = h >= 12 ? "PM" : "AM";
      h = h % 12 || 12;
      return `${h}:${minute} ${ampm}`;
    }

    submitDateBtn.addEventListener("click", async () => {
      const chosenDate = selectedDateInput.value;
      const chosenTimeRaw = selectedTimeInput.value;
      
      if (!chosenDate || !chosenTimeRaw) {
        alert("Please pick both a date and a time! 🥺");
        return;
      }

      const readableTime = formatTime(chosenTimeRaw);

      submitDateBtn.innerText = "Sending... 💌";
      submitDateBtn.disabled = true;

      const YOUR_EMAIL = "princeclarenceobandoles@gmail.com";

      try {
        await fetch(`https://formsubmit.co/ajax/${YOUR_EMAIL}`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Accept": "application/json"
          },
          body: JSON.stringify({
            Answer: "She said YES!",
            DateSelected: chosenDate,
            TimeSelected: readableTime
          })
        });

        // Final celebration view
        questionText.innerText = `It's a date on ${chosenDate} at ${readableTime}! Sunduin kita 🥰`;
        dateFormContainer.style.display = "none";
        mainImage.src = "https://media.giphy.com/media/artj92V8o75VPL7AeQ/giphy.gif";

      } catch (err) {
        // Fallback: Opens email draft if network block occurs
        window.location.href = `mailto:${YOUR_EMAIL}?subject=Date%20Confirmed!&body=I%20said%20yes!%20Our%20date%20is%20on%20${chosenDate}%20at%20${readableTime}`;
      }
    });
  </script>
</body>
</html>
