Request:

I want you to create a website for my new band, Jellytoes.  The background of the home page is entirely the attached image.  Users must be able to interact with the image.  When they hover their cursor over each of the people in the image, the will be a faint glow effect (consonant with colors in the image) around the people to invite the user to click.  When the user clicks the guitarist on the left, you go to the bio for Matt.  When the user clicks the drummer you will go to the bio for Dave.  When the user clicks the guitarist on the right, you will go to the bio for Alex.  When the user clicks on the person watching the band, you will invite them to become a fan by signing up for the Jellytoes mailing list.  This must be extremely professionally done.






Answer:

Below is a simple, fully self‐contained example of how you might build a one‐page site for **Jellytoes** that uses the provided image as the homepage background, allows hovering over each band member (plus the audience member), applies a faint glow on hover, and navigates to the respective bios or fan signup. The code is annotated so you can see how to adapt and polish it further for your professional needs.

> **Note:**  
> 1. For demonstration, the code uses approximate positioning for the clickable “hotspots.” In production, you’ll fine‐tune these positions so the glow perfectly aligns with each person.  
> 2. The bios (Matt, Dave, Alex) are shown as separate pages (`matt.html`, `dave.html`, `alex.html`), but you could route them any way you wish (for instance, as separate sections on the same page, or in modals).  
> 3. The fan signup is shown in a simple modal dialog for illustration. Customize as needed for your actual mailing list system.  
> 4. Replace `YOUR_IMAGE_PATH` with the actual path to your band image.

---

## **HTML (index.html)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Jellytoes – Official Band Site</title>
  <!-- Example of including a Google Font -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <!-- Main Container -->
  <div class="hero-container">
    <!-- The background is handled in CSS; 
         we place a transparent overlay for color adjustments if desired. -->
    <div class="hero-overlay"></div>

    <!-- Hotspots: each anchor (or div) is absolutely positioned over a person. -->
    <a href="matt.html" class="hotspot hotspot-left" data-member="Matt"></a>
    <a href="dave.html" class="hotspot hotspot-center" data-member="Dave"></a>
    <a href="alex.html" class="hotspot hotspot-right" data-member="Alex"></a>
    <div class="hotspot hotspot-audience" data-member="Become a Fan"></div>

    <!-- Logo / Band Name (optional) -->
    <div class="band-logo">
      <h1>Jellytoes</h1>
      <p>Progressive Rock | Fusion | Jam</p>
    </div>
  </div>

  <!-- Hidden modal for fan sign-up, shown when clicking the “hotspot-audience.” -->
  <div class="fan-modal-overlay" id="fanModalOverlay">
    <div class="fan-modal">
      <button class="close-btn" id="closeModal">×</button>
      <h2>Join the Jellytoes Mailing List</h2>
      <p>Sign up to get the latest updates, show dates, and exclusive content from Jellytoes.</p>
      <form action="#" method="post">
        <input type="text" name="fullname" placeholder="Your Full Name" required />
        <input type="email" name="email" placeholder="Your Email" required />
        <button type="submit">Sign Up</button>
      </form>
    </div>
  </div>

  <!-- Optional JavaScript to handle the fan sign-up modal -->
  <script>
    // Toggle fan sign-up modal
    const audienceHotspot = document.querySelector('.hotspot-audience');
    const fanModalOverlay = document.getElementById('fanModalOverlay');
    const closeModalBtn = document.getElementById('closeModal');

    // Show the modal
    audienceHotspot.addEventListener('click', () => {
      fanModalOverlay.classList.add('active');
    });

    // Hide the modal
    closeModalBtn.addEventListener('click', () => {
      fanModalOverlay.classList.remove('active');
    });

    // Click outside to close modal (optional)
    fanModalOverlay.addEventListener('click', (event) => {
      if (event.target === fanModalOverlay) {
        fanModalOverlay.classList.remove('active');
      }
    });
  </script>
</body>
</html>
```

---

## **CSS (styles.css)**

```css
/* Reset some default browser styling */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Lato', sans-serif;
  color: #fff;
  background-color: #000; /* Fallback if image doesn't load */
  overflow-x: hidden;
  overflow-y: auto;
}

/* Hero Container – the page “header” or “home” section */
.hero-container {
  position: relative;
  width: 100%;
  height: 100vh; /* Full screen height */
  background: url("YOUR_IMAGE_PATH") no-repeat center center/cover;
  /* If you have a large, high-res image, consider using background-size: cover; */
}

/* Optional overlay that could tint the background if you want a subtle overlay */
.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* Example subtle overlay color; adjust or remove if not needed */
  background: rgba(0, 0, 0, 0.2);
}

/* Band Logo / Title area (optional) */
.band-logo {
  position: absolute;
  top: 5%;
  left: 5%;
  z-index: 10;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
}

.band-logo h1 {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.band-logo p {
  font-size: 1.2rem;
  font-weight: 300;
}

/* Hotspots
   Each hotspot is an invisible clickable area over the person in the image.
   We'll give them approximate widths/heights to form a bounding box.
   Adjust top/left/width/height to match the exact location of each person. */
.hotspot {
  position: absolute;
  cursor: pointer;
  /* Transparent background initially, but we need something to make the glow visible on hover. */
  background-color: rgba(255, 255, 255, 0);
  transition: box-shadow 0.3s ease, background-color 0.3s ease;
}

/* Left Guitarist (Matt) approximate bounding box */
.hotspot-left {
  top: 40%;
  left: 20%;
  width: 10%;
  height: 25%;
}

/* Drummer (Dave) approximate bounding box */
.hotspot-center {
  top: 40%;
  left: 45%;
  width: 10%;
  height: 25%;
}

/* Right Guitarist (Alex) approximate bounding box */
.hotspot-right {
  top: 40%;
  left: 68%;
  width: 10%;
  height: 25%;
}

/* Audience member approximate bounding box */
.hotspot-audience {
  top: 60%;
  left: 10%;
  width: 10%;
  height: 20%;
}

/* Glow effect on hover */
.hotspot:hover {
  box-shadow: 0 0 20px 5px rgba(255, 255, 255, 0.4);
  background-color: rgba(255, 255, 255, 0.05); /* faint highlight over the person */
}

/* ---------------------
   Fan Signup Modal
   --------------------- */
.fan-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease;
  z-index: 999; /* on top of everything */
}

.fan-modal-overlay.active {
  opacity: 1;
  visibility: visible;
}

.fan-modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #111;
  padding: 2rem;
  border-radius: 6px;
  width: 90%;
  max-width: 500px;
}

.fan-modal h2 {
  margin-bottom: 1rem;
  font-size: 1.8rem;
  text-align: center;
}

.fan-modal p {
  margin-bottom: 1.5rem;
  text-align: center;
  line-height: 1.5;
}

.fan-modal form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.fan-modal input {
  padding: 0.8rem;
  font-size: 1rem;
  border: none;
  border-radius: 4px;
}

.fan-modal button[type="submit"] {
  background: #bd1818;
  color: #fff;
  font-size: 1rem;
  padding: 0.8rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s ease;
}

.fan-modal button[type="submit"]:hover {
  background: #a21616;
}

.close-btn {
  position: absolute;
  top: 0.5rem;
  right: 0.7rem;
  background: transparent;
  border: none;
  font-size: 2rem;
  color: #fff;
  cursor: pointer;
}
```

---

## **Bio Pages (Example)**

Create separate HTML files for Matt, Dave, and Alex. For instance, `matt.html` could look like this (repeat the pattern for Dave and Alex, substituting their text and images):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Jellytoes – Matt’s Bio</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <div class="bio-container">
    <h1>Matt – Bassist & Mellotronnier</h1>
    <p>
      Matt, the self-proclaimed "Bassist and Mellotronnier" of Jelly Toes, is a musical enigma...
      <!-- (Use the full text from your provided bio) -->
    </p>

    <!-- Link back to home page or site navigation -->
    <p><a href="index.html">← Back to Jellytoes Home</a></p>
  </div>
</body>
</html>
```

---

### **How It Works**

1. **Full‐Screen Background**  
   - The `body` background is black as a fallback.  
   - The `.hero-container` uses `background: url("YOUR_IMAGE_PATH")` to set the entire viewport with the band image.  

2. **Interactivity / Hotspots**  
   - Each `<a class="hotspot ...">` or `<div class="hotspot ...">` is absolutely positioned over a person in the background image.  
   - On hover, a faint glow (`box-shadow`) is revealed, inviting the user to click.  
   - The anchor tags for the left guitarist, center drummer, and right guitarist link to separate bio pages.  
   - The audience member hotspot is a `<div>` so it can trigger JavaScript for the sign‐up modal.  

3. **Fan Signup Modal**  
   - When the user clicks the “audience hotspot,” we set `.fan-modal-overlay` to its “active” state, making it visible.  
   - The user can close the modal by clicking the “×” button or clicking outside the modal.  

4. **Professional Look & Feel**  
   - Use any branding or design you like: a logo, color scheme, fonts, or additional layout sections.  
   - Tweak the glow color, intensities, and transitions for a subtler or more dramatic effect.  

5. **Further Enhancements**  
   - Add navigation, music player, social links, streaming embed, or event schedule.  
   - Make the site responsive by refining the hotspot positions at various breakpoints if the image or layout shifts significantly on mobile vs. desktop.  

With this setup, you’ll have a **clean, one-page home screen** for **Jellytoes** featuring an immersive full-background band image, interactive hotspots with hover glows, and navigable band member bios or fan signups—all while maintaining a polished, professional aesthetic.