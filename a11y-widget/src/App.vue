<template>
  <div id="app">
    <header class="navbar">
      <div class="logo">AccessWidget</div>
      <nav class="nav-links">
        <a href="#">Home</a>
        <a href="#features">Features</a>
        <a href="#about">About</a>
        <a href="#testimonials">Testimonials</a>
        <a href="#contact">Contact</a>
      </nav>
    </header>

    <FloatingButton />

    <main class="demo-content">
      <h1>Welcome to the Accessibility Widget Demo</h1>
      <p>
        This paragraph demonstrates the default text. You can adjust its size,
        spacing, or switch to a dyslexia-friendly font using the widget.
      </p>
      <p>
        Here is a <a href="#">sample link</a> to test the
        <strong>Highlight Links</strong> option.
      </p>

      <img
        src="./assets/tab.jpg"
        alt="Demo Screenshot"
      />

      <section id="features">
        <h2>Features</h2>
        <ul>
          <li>Resize font dynamically</li>
          <li>Switch to high contrast mode</li>
          <li>Enable dyslexia-friendly fonts</li>
          <li>Hide non-essential images</li>
          <li>Pause CSS animations</li>
          <li>Highlight all clickable links</li>
          <li>Enlarge or replace cursor</li>
        </ul>
      </section>

      <section id="about">
        <h2>About This Tool</h2>
        <p>
          This accessibility widget is designed to help improve web experiences
          for users with diverse needs. Built using Vue 3, it allows you to
          toggle multiple assistive features seamlessly.
        </p>
        <p>
          We follow best practices from WCAG and ARIA guidelines to ensure
          inclusivity and usability for all users.
        </p>
      </section>

      <section id="testimonials">
        <h2>Testimonials</h2>
        <blockquote>
          "This tool made our site instantly more accessible without rewriting
          the whole frontend. Highly recommend!" – Dev Team @ BrightTech
        </blockquote>
        <blockquote>
          "As a dyslexic user, the ability to switch fonts and adjust spacing
          helped me navigate content way better." – Maya K.
        </blockquote>
      </section>

      <section id="contact">
        <h2>Contact Us</h2>
        <p>Have questions or feature requests? Reach out via email:</p>
        <a href="mailto:support@accesswidget.com">support@accesswidget.com</a>
      </section>
    </main>
  </div>
</template>

<script setup>
import FloatingButton from './components/FloatingButton.vue';
</script>

<style scoped>
/* Navbar Styles */
.navbar {
  background: #1a1a1a;
  opacity: 0.9;
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  position: sticky;
  top: 0;
  z-index: 100;
  margin: 0;
}

.logo {
  font-weight: bold;
  font-size: 1.5rem;
}

.nav-links a {
  color: white;
  text-decoration: none;
  margin-left: 1.5rem;
  transition: color 0.2s;
}

.nav-links a:hover {
  color: #00bcd4;
}

/* Main content styles */
.demo-content {
  max-width: 900px;
  margin: 2rem auto;
  padding: 2rem;
  line-height: 1.7;
  background-color: #f9f9f9;
  border-radius: 10px;
}

.demo-content h1,
h2 {
  margin-bottom: 1rem;
  color: #222;
}

.demo-content p {
  margin-bottom: 1.2rem;
}

.demo-content img {
  display: block;
  margin: 1.5rem auto;
  width: 100%;
  max-width: 500px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

blockquote {
  background: #e3f2fd;
  border-left: 4px solid #2196f3;
  padding: 1rem;
  margin: 1rem 0;
  font-style: italic;
}

.demo-content img {
  display: block;
  margin: 1.5rem auto;
  width: 100%;
  max-width: 990px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(255, 255, 255, 0.1);
}
</style>
