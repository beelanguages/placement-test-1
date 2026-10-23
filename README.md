<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Bee Languages · Placement Test</title>
  <meta name="description" content="A personalized English placement test by Bee Languages." />
  <link rel="icon" type="image/png" href="assets/bee-logo-64.png" />
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header class="topbar">
    <div class="brand">
      <img class="bee-logo" src="assets/bee-logo-64.png" alt="" width="32" height="32" aria-hidden="true" />
      <span class="brand-name">Bee Languages</span>
    </div>
    <div class="lang-toggle" role="group" aria-label="Instructions language">
      <button type="button" id="lang-en" class="chip is-active" aria-pressed="true">EN</button>
      <button type="button" id="lang-pt" class="chip" aria-pressed="false">PT</button>
    </div>
  </header>

  <main id="app" class="container">
    <!-- Screens are rendered by app.js -->
    <noscript>
      <p class="notice">This test needs JavaScript enabled to run.</p>
    </noscript>
  </main>

  <footer class="footer">
    <p>Bee Languages · Independent placement assessment. Not an official international examination or certificate.</p>
  </footer>

  <script src="data/questions.js"></script>
  <script src="data/skills.js"></script>
  <script src="app.js"></script>
</body>
</html>
