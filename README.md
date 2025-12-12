<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Advanced Resume Builder</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet" />
  <style>
    body { font-family: 'Poppins', sans-serif; margin: 0; background: #eef1f5; color: #333; }
    header { background: #2c3e50; color: #fff; padding: 1rem; text-align: center; font-size: 1.8rem; }
    .container { max-width: 1200px; margin: 20px auto; padding: 20px; background: #fff; border-radius: 10px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
    label { margin-top: 1rem; display: block; font-weight: 600; }
    input, textarea { width: 100%; padding: 10px; margin-top: 6px; border: 1px solid #ccc; border-radius: 6px; }
    textarea { resize: vertical; }
    button { margin-top: 20px; background: #3498db; color: #fff; padding: 10px 20px; border: none; border-radius: 6px; font-weight: 600; cursor: pointer; }
    button:hover { background: #2980b9; }
    .section-add { margin-top: 10px; display: inline-block; background: #2ecc71; color: white; padding: 6px 12px; border-radius: 6px; cursor: pointer; font-size: 0.9rem; }
    .template-buttons div { display: inline-block; padding: 10px 16px; border: 2px solid #ccc; margin: 10px 10px 0 0; border-radius: 8px; cursor: pointer; }
    .template-buttons .selected { background: #2ecc71; color: white; border-color: #27ae60; }
    .entry-block { border: 1px solid #ccc; padding: 10px; border-radius: 6px; margin-top: 10px; position: relative; }
    .entry-block button { position: absolute; top: 5px; right: 5px; background: red; color: white; font-size: 0.8rem; padding: 4px 8px; }
  </style>
</head>
<body>
  <header>📝 Advanced Resume Builder</header>
  <div class="container">
    <form id="resumeForm">
      <div class="form-grid">
        <div>
          <label>Full Name *</label><input type="text" id="name" required>
          <label>Email *</label><input type="email" id="email" required>
          <label>Phone *</label><input type="text" id="phone" required>
          <label>Address</label><textarea id="address"></textarea>
          <label>LinkedIn</label><input type="text" id="linkedin">
          <label>Portfolio</label><input type="text" id="portfolio">
        </div>
        <div>
          <label>Summary</label><textarea id="summary" rows="4"></textarea>
          <label>Skills (comma separated)</label><input type="text" id="skills">
          <label>Hobbies (comma separated)</label><input type="text" id="hobbies">

          <label>Education</label>
          <div id="education-section"></div>
          <div class="section-add" onclick="addBlock('education-section')">+ Add Education</div>

          <label>Experience</label>
          <div id="experience-section"></div>
          <div class="section-add" onclick="addBlock('experience-section')">+ Add Experience</div>

          <label>Projects</label>
          <div id="projects-section"></div>
          <div class="section-add" onclick="addBlock('projects-section')">+ Add Project</div>
        </div>
      </div>

      <h3>Select Template</h3>
      <div class="template-buttons">
        <div class="selected" onclick="selectTemplate(this, 'simple')">Simple</div>
        <div onclick="selectTemplate(this, 'modern')">Modern</div>
        <div onclick="selectTemplate(this, 'classic')">Classic</div>
      </div>

      <button type="button" onclick="generateResume()">Generate Resume</button>
    </form>
  </div>
   </div>
        <button class="btn print-btn" onclick="window.print()">Save / Print PDF</button>
  </div>

  <script>
    let selectedTemplate = 'simple';

    function selectTemplate(el, type) {
      document.querySelectorAll('.template-buttons div').forEach(btn => btn.classList.remove('selected'));
      el.classList.add('selected');
      selectedTemplate = type;
    }

    function addBlock(sectionId) {
      const section = document.getElementById(sectionId);
      const div = document.createElement('div');
      div.className = 'entry-block';
      div.innerHTML = `
        <input type="text" placeholder="Title (e.g., B.Tech in CSE)" required />
        <textarea placeholder="Details (e.g., University, Duration)" rows="2"></textarea>
        <button type="button" onclick="this.parentElement.remove()">×</button>
      `;
      section.appendChild(div);
    }

    function generateResume() {
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const phone = document.getElementById('phone').value;
      const address = document.getElementById('address').value;
      const linkedin = document.getElementById('linkedin').value;
      const portfolio = document.getElementById('portfolio').value;
      const summary = document.getElementById('summary').value;
      const skills = document.getElementById('skills').value.split(',').map(x => x.trim()).filter(Boolean);
      const hobbies = document.getElementById('hobbies').value.split(',').map(x => x.trim()).filter(Boolean);

      function getEntries(sectionId) {
        return [...document.getElementById(sectionId).children].map(div => {
          const inputs = div.querySelectorAll('input, textarea');
          return <strong>${inputs[0].value}</strong><br>${inputs[1].value};
        }).join('<br><br>');
      }

      const education = getEntries('education-section');
      const experience = getEntries('experience-section');
      const projects = getEntries('projects-section');

      const resumeHTML = `
<!DOCTYPE html>
<html>
<head>
  <title>${name}'s Resume</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Poppins', sans-serif; background: #fff; margin: 0; padding: 40px; }
    .resume { max-width: 800px; margin: auto; padding: 40px; border: 1px solid #ccc; box-shadow: 0 0 10px rgba(0,0,0,0.1); background: #fff; }
    h2 { margin: 0; color: #2c3e50; }
    .contact-info { margin-bottom: 20px; font-size: 0.95rem; }
    .section { margin-top: 30px; }
    .section h3 { margin-bottom: 10px; color: #2c3e50; border-bottom: 1px solid #ccc; padding-bottom: 6px; }
    ul { padding-left: 20px; margin: 0; }
    a { color: #2980b9; text-decoration: none; }
  </style>
</head>
<body>
  <div class="resume">
    <h2>${name}</h2>
    <div class="contact-info">
      <p>${email} | ${phone}</p>
      <p>${address}</p>
      <p>${linkedin ? <a href="https://${linkedin}" target="_blank">LinkedIn</a> : ''} ${portfolio ? | <a href="https://${portfolio}" target="_blank">Portfolio</a> : ''}</p>
    </div>

    <div class="section">
      <h3>Summary</h3>
      <p>${summary}</p>
    </div>

    <div class="section">
      <h3>Education</h3>
      <p>${education}</p>
    </div>

    <div class="section">
      <h3>Experience</h3>
      <p>${experience}</p>
    </div>

    <div class="section">
      <h3>Projects</h3>
      <p>${projects}</p>
    </div>

    <div class="section">
      <h3>Skills</h3>
      <ul>${skills.map(skill => <li>${skill}</li>).join('')}</ul>
    </div>

    <div class="section">
      <h3>Hobbies</h3>
      <ul>${hobbies.map(hobby => <li>${hobby}</li>).join('')}</ul>
    </div>
    </div>
      </div>
         <button class="btn print-btn"  "background=#27ae60" onclick="window.print()">Save / Print PDF</button>
  </div>

</body>
</html>
      `;

      const win = window.open('', '_blank');
      win.document.write(resumeHTML);
      win.document.close();
    }
  </script>
</body>
</html>
