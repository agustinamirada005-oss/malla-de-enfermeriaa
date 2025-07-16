// script.js

const malla = [
  {
    anio: "Primer Año",
    semestre: "Primer Semestre",
    asignaturas: [
      { nombre: "Bases del cuidado en enfermería", requisitos: [] },
      { nombre: "Salud pública y política sanitaria", requisitos: [] },
      { nombre: "Enfermería comunitaria", requisitos: ["Bases del cuidado en enfermería", "Problemática sociocultural y de la salud", "Comunicación en ciencia de la salud"] },
      { nombre: "Morfo fisiología dinámica humana", requisitos: [] },
      { nombre: "Física o química aplicada", requisitos: [] },
      { nombre: "Aspectos Psicosociales y culturales del desarrollo", requisitos: [] },
      { nombre: "Problemática sociocultural y de la salud", requisitos: [] },
      { nombre: "Entornos virtuales de información y comunicación", requisitos: [] },
      { nombre: "Comunicación en ciencia de la salud", requisitos: [] },
      { nombre: "Práctica profesionalizarte 1", requisitos: ["Enfermería comunitaria", "Salud pública y política sanitaria", "Bases del cuidado en enfermería"] }
    ]
  },
  {
    anio: "Segundo Año",
    semestre: "Segundo Semestre",
    asignaturas: [
      { nombre: "Nutrición y dietoterapia", requisitos: ["Morfo fisiología dinámica humana", "Física o química aplicada"] },
      { nombre: "Psicología Social y organizacional", requisitos: ["Problemática sociocultural y de la salud"] },
      { nombre: "Microbiología parasitología e inmunología", requisitos: ["Morfo fisiología dinámica humana", "Física o química aplicada"] },
      { nombre: "Epidemiología y bioestadística", requisitos: ["Salud pública y política sanitaria", "Enfermería comunitaria"] },
      { nombre: "Farmacología aplicada", requisitos: ["Morfo fisiología dinámica humana", "Física o química aplicada"] },
      { nombre: "Investigación en enfermería", requisitos: ["Enfermería comunitaria", "Comunicación en ciencia de la salud"] },
      { nombre: "Educación para la salud", requisitos: ["Enfermería comunitaria", "Comunicación en ciencia de la salud"] },
      { nombre: "Cuidados de enfermería en el adulto", requisitos: ["Práctica profesionalizarte 1", "Morfo fisiología dinámica humana", "Física o química aplicada"] },
      { nombre: "Cuidados de enfermería en el adulto mayor", requisitos: ["Práctica profesionalizarte 1", "Morfo fisiología dinámica humana", "Física o química aplicada", "Cuidados de enfermería en el adulto"] },
      { nombre: "Prácticas profesionalizantes 2", requisitos: ["Práctica profesionalizarte 1", "Nutrición y dietoterapia", "Epidemiología y bioestadística", "Farmacología aplicada", "Cuidados de enfermería en el adulto", "Cuidados de enfermería en el adulto mayor"] }
    ]
  },
  {
    anio: "Tercer Año",
    semestre: "Tercer Semestre",
    asignaturas: [
      { nombre: "Inglés técnico", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Ética y legislación aplicada", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Cuidado en emergencias", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Cuidado en salud mental", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Gestión y autogestión en enfermería", requisitos: ["Psicología Social y organizacional"] },
      { nombre: "Cuidados de la enfermería materna y del recién nacido", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Cuidado en enfermería del niño y del adolescente", requisitos: ["Prácticas profesionalizantes 2"] },
      { nombre: "Prácticas profesionalizantes 3", requisitos: ["Inglés técnico", "Ética y legislación aplicada", "Cuidado en emergencias", "Cuidado en salud mental", "Gestión y autogestión en enfermería", "Cuidados de la enfermería materna y del recién nacido", "Cuidado en enfermería del niño y del adolescente"] }
    ]
  }
];

const aprobadas = new Set();

function actualizarEstado() {
  document.querySelectorAll('.asignatura').forEach(div => {
    const nombre = div.dataset.nombre;
    const requisitos = JSON.parse(div.dataset.requisitos);
    const boton = div.querySelector('button');

    const habilitada = requisitos.every(r => aprobadas.has(r));
    boton.disabled = !habilitada && !aprobadas.has(nombre);
  });
}

function crearMalla() {
  const contenedor = document.getElementById("malla");

  malla.forEach(bloque => {
    const divSemestre = document.createElement("div");
    divSemestre.className = "semestre";
    divSemestre.innerHTML = `<h2>${bloque.anio} - ${bloque.semestre}</h2>`;

    bloque.asignaturas.forEach(asig => {
      const divAsig = document.createElement("div");
      divAsig.className = "asignatura";
      divAsig.dataset.nombre = asig.nombre;
      divAsig.dataset.requisitos = JSON.stringify(asig.requisitos);

      const spanNombre = document.createElement("span");
      spanNombre.textContent = asig.nombre;

      const btn = document.createElement("button");
      btn.textContent = "Aprobar";
      btn.className = "boton-aprobado";

      btn.addEventListener("click", e => {
        e.stopPropagation();
        aprobadas.add(asig.nombre);
        divAsig.classList.add("aprobada");
        btn.disabled = true;
        actualizarEstado();
      });

      divAsig.appendChild(spanNombre);
      divAsig.appendChild(btn);
      divSemestre.appendChild(divAsig);
    });

    contenedor.appendChild(divSemestre);
  });

  actualizarEstado();
}

crearMalla();
