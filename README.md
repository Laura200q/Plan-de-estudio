<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Malla Curricular - Checklist</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>Malla Curricular - Licenciatura en Enfermería (UNSa)</h1>
  <p>Marca las materias que ya cursaste para llevar tu progreso.</p>

  <div id="checklist"></div>

  <button id="resetBtn">Reiniciar checklist</button>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 40px auto;
  padding: 0 20px;
  background: #f9f9f9;
  color: #333;
}

h1 {
  text-align: center;
  margin-bottom: 0;
}

#checklist {
  margin-top: 20px;
}

.year-section {
  margin-bottom: 30px;
}

.year-section h2 {
  border-bottom: 2px solid #007BFF;
  padding-bottom: 4px;
  color: #007BFF;
}

.materia {
  display: flex;
  align-items: center;
  margin: 8px 0;
}

.materia input[type="checkbox"] {
  margin-right: 12px;
  width: 18px;
  height: 18px;
  cursor: pointer;
}

button#resetBtn {
  display: block;
  margin: 30px auto 0;
  background: #dc3545;
  color: white;
  border: none;
  padding: 10px 18px;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.3s ease;
}

button#resetBtn:hover {
  background: #b02a37;
}
const malla = {
  "1er Año": [
    "Anatomía Humana",
    "Bioquímica",
    "Psicología General",
    "Fisiología",
    "Ciencias Sociales I",
    "Introducción a la Enfermería",
    "Práctica Hospitalaria I",
  ],
  "2do Año": [
    "Microbiología y Parasitología",
    "Enfermería Médica I",
    "Enfermería en Salud Pública",
    "Epidemiología",
    "Enfermería en Clínica Médica",
    "Ciencias Sociales II",
    "Práctica Hospitalaria II",
  ],
  "3er Año": [
    "Enfermería Quirúrgica",
    "Enfermería Pediátrica",
    "Enfermería en Salud Mental",
    "Enfermería Obstétrica",
    "Bioética y Legislación",
    "Práctica Profesional Integrada I",
  ],
  "4to Año": [
    "Administración en Servicios de Salud",
    "Investigación en Enfermería I",
    "Docencia en Enfermería",
    "Inglés Técnico",
    "Práctica Profesional Integrada II",
  ],
  "5to Año": [
    "Supervisión de Enfermería",
    "Ética Profesional",
    "Investigación en Enfermería II",
    "Práctica Profesional Integrada III",
    "Seminario de Tesis o Trabajo Final",
  ],
};

const checklistDiv = document.getElementById("checklist");
const STORAGE_KEY = "mallaEnfermeriaChecklist";

function saveProgress(progress) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(progress));
}

function loadProgress() {
  const saved = localStorage.getItem(STORAGE_KEY);
  return saved ? JSON.parse(saved) : {};
}

function createChecklist() {
  const progress = loadProgress();

  for (const [year, materias] of Object.entries(malla)) {
    const yearSection = document.createElement("div");
    yearSection.className = "year-section";

    const yearTitle = document.createElement("h2");
    yearTitle.textContent = year;
    yearSection.appendChild(yearTitle);

    materias.forEach((materia) => {
      const materiaDiv = document.createElement("div");
      materiaDiv.className = "materia";

      const checkbox = document.createElement("input");
      checkbox.type = "checkbox";
      checkbox.id = `${year}-${materia}`;
      checkbox.checked = progress[checkbox.id] || false;

      checkbox.addEventListener("change", () => {
        progress[checkbox.id] = checkbox.checked;
        saveProgress(progress);
      });

      const label = document.createElement("label");
      label.htmlFor = checkbox.id;
      label.textContent = materia;

      materiaDiv.appendChild(checkbox);
      materiaDiv.appendChild(label);
      yearSection.appendChild(materiaDiv);
    });

    checklistDiv.appendChild(yearSection);
  }
}

function resetChecklist() {
  if (confirm("¿Querés reiniciar tu checklist? Se borrará tu progreso guardado.")) {
    localStorage.removeItem(STORAGE_KEY);
    checklistDiv.innerHTML = "";
    createChecklist();
  }
}

document.getElementById("resetBtn").addEventListener("click", resetChecklist);

createChecklist();