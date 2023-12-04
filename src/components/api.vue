<script setup>
import { onMounted, ref } from 'vue';
import moment from 'moment';

const info = ref('');
const info2 = ref('');

async function fetchData(apiUrl, dataRef) {
  try {
    const response = await fetch(apiUrl);
    const data = await response.json();
    dataRef.value = data;
    console.log(dataRef.value);
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}

function formatFecha(fecha) {
  return moment(fecha).format('DD [de] MM [del] YYYY'); // Cambié el formato a 'DD/MM/YYYY'
}

onMounted(async () => {
  // Llamada al primer API
  await fetchData('https://sitmotul.com.mx/api/statusEval', info);

  // Llamada al segundo API
  await fetchData('https://sitmotul.com.mx/api/statusEvalIng', info2);
});

const sistemasData = ref({
    listas: 0,
    faltantes: 0
});
const electromecanicaData = ref({
    listas: 0,
    faltantes: 0
});

const renovablesData = ref({
    listas: 0,
    faltantes: 0
});

const electronicaData = ref({
    listas: 0,
    faltantes: 0
});

const industrialData = ref({
    listas: 0,
    faltantes: 0
});

const fetchData2 = async () => {
    try {
        const response = await fetch('https://sitmotul.com.mx/api/statusEvalIng');
        const data = await response.json();

        // Verifica que la propiedad "ISC" exista en los datos
        if (data.ISC) {
          sistemasData.value.listas = data.ISC.listas;
          sistemasData.value.faltantes = data.ISC.faltantes;
            electromecanicaData.value.listas = data.IEM.listas;
            electromecanicaData.value.faltantes = data.IEM.faltantes;
            renovablesData.value.listas = data.IER.listas;
            renovablesData.value.faltantes = data.IER.faltantes;
            electronicaData.value.listas = data.IE.listas;
            electronicaData.value.faltantes = data.IE.faltantes;
            industrialData.value.listas = data.II.listas;
            industrialData.value.faltantes = data.II.faltantes;
            console.log(sistemasData.value.listas);
        } else {
            console.error('La propiedad no está presente en los datos recibidos:', data);
        }
    } catch (error) {
        console.error('Error al obtener datos:', error);
    }
};

onMounted(() => {
    fetchData2();
});

function calculateHoursRemaining(deadline) {
  const fechaLimite = new Date(deadline);
  const ahora = new Date();

  // Calcular la diferencia en horas
  const diferenciaHoras = Math.max(0, Math.floor((fechaLimite - ahora) / (1000 * 60 * 60)));

  return `${diferenciaHoras} `;
}



const countdownDate = ref(new Date("Dec 31, 2023 00:00:00").getTime());
const hours = ref(0);
const distance = ref(0);
const updateCountdown = () => {
  const now = new Date().getTime();
  distance.value = countdownDate.value - now;
  hours.value = Math.floor((distance.value % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

};

const formattedTime = (time) => {
  return time < 10 ? "0" + time : time;
};

onMounted(() => {
  updateCountdown();
  setInterval(updateCountdown, 1000);
});
</script>


<template>
  
  <!-- Encabezado -->
<div class="flex flex-col justify-center items-center bg-[#f7f4e8] py-5">
  <img class="h-[100px] w-[100px]" src="./logo.png" alt="">
  <h1 class="text-xl">SITMOTUL</h1>
  <h2 class="text-center">Estado de la evaluación tutor del {{ info.periodo }} al Martes 28 de Noviembre de 2023</h2>
</div>

<!-- Cuerpo principal -->
<div class="flex justify-center items-center h-[644px] md:h-screen bg-[#f7f4e8] text-xs py-2">
  <!-- Tres divs a la izquierda -->
  <div class="flex flex-col">
    <!-- Sistemas Computacionales -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-3 rounded-full bg-gradient-to-r bg-[#badbcc] mb-2 md:mb-4 text-center">
      <h2>Sistemas Computacionales</h2>
      <i class="fa-solid fa-desktop p-1"></i>
      <div class="flex items-center flex-col p-2">
        <div class="relative">
          <div class="rounded-full border border-black w-16 h-16 relative overflow-hidden">
            <div class="absolute bottom-0 left-0 w-full bg-yellow-500 text-white flex items-center justify-center" :style="{ height: `${sistemasData.listas / (sistemasData.listas + sistemasData.faltantes) * 100}%` }">
              {{ Math.round(sistemasData.listas / (sistemasData.listas + sistemasData.faltantes) * 100) }}%
            </div>
          </div>
        </div>
      </div>
      <h3>Completados: {{ sistemasData.listas }}</h3>
      <h3>Faltantes: {{ sistemasData.faltantes }}</h3>
    </div>

    <!-- Electromecánica -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-3 rounded-full bg-[#badbcc] mb-2 md:mb-4">
      <h2>Electromecánica</h2>
      <i class="fa-solid fa-plug-circle-exclamation p-1"></i>
      <div class="flex items-center flex-col p-2">
        <div class="relative">
          <div class="rounded-full border border-black w-16 h-16 relative overflow-hidden">
            <div class="absolute bottom-0 left-0 w-full bg-yellow-500 text-white flex items-center justify-center" :style="{ height: `${electromecanicaData.listas / (electromecanicaData.listas + electromecanicaData.faltantes) * 100}%` }">
              {{ Math.round(electromecanicaData.listas / (electromecanicaData.listas + electromecanicaData.faltantes) * 100) }}%
            </div>
          </div>
        </div>
      </div>
      <h3>Completados: {{ electromecanicaData.listas }}</h3>
      <h3>Faltantes: {{ electromecanicaData.faltantes }}</h3>
    </div>

    <!-- Electrónica -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-3 rounded-full bg-[#badbcc] mb-2 md:mb-4">
      <h2>Electrónica</h2>
      <i class="fa-solid fa-bolt p-1"></i>
      <div class="flex items-center flex-col p-2">
        <div class="relative">
          <div class="rounded-full border border-black w-16 h-16 relative overflow-hidden">
            <div class="absolute bottom-0 left-0 w-full bg-yellow-500 text-white flex items-center justify-center" :style="{ height: `${electronicaData.listas / (electronicaData.listas + electronicaData.faltantes) * 100}%` }">
              {{ Math.round(electronicaData.listas / (electronicaData.listas + electronicaData.faltantes) * 100) }}%
            </div>
          </div>
        </div>
      </div>
      <h3>Completados: {{ electronicaData.listas }}</h3>
      <h3>Faltantes: {{ electronicaData.faltantes }}</h3>
    </div>
  </div>

  <!-- Div en el medio -->
  <div class="mr-2 ml-2 md:ml-7 md:mr-7">
    <div class="flex flex-col justify-center w-28 h-28 md:w-56 md:h-56 rounded-full bg-[#293f38] text-white">
      <p class="text-center">Tiempo para Finalizar</p>
      <p class="text-center md:text-4xl text-xl">{{ calculateHoursRemaining(info.fin) }} Horas</p>
    </div>
  </div>

  <!-- Tres divs a la derecha -->
  <div class="flex flex-col items-center">
    <!-- Energías Renovables -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-3 rounded-full bg-[#badbcc] mb-2 md:mb-4 text-center">
      <h2>Energias Renovables</h2>
      <i class="fa-solid fa-charging-station p-1"></i>
      <div class="flex items-center flex-col p-2">
        <div class="relative">
          <div class="rounded-full border border-black w-16 h-16 relative overflow-hidden">
            <div class="absolute bottom-0 left-0 w-full bg-yellow-500 text-white
            flex items-center justify-center" :style="{ height: `${renovablesData.listas / (renovablesData.listas + renovablesData.faltantes) * 100}%` }">
               {{ Math.round(renovablesData.listas / (renovablesData.listas + renovablesData.faltantes) * 100) }}%
              </div>
           </div>
          </div>
        </div>
         <h3>Completados: {{ renovablesData.listas }}</h3>
         <h3>Faltantes: {{ renovablesData.faltantes }}</h3>
    </div>
  
    <!-- Industrial -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-3 rounded-full bg-[#badbcc] mb-2 md:mb-4">
      <h2>Industrial</h2>
      <i class="fa-solid fa-sheet-plastic p-1"></i>

      <div class="flex items-center flex-col p-2">
        <div class="relative">
          <div class="rounded-full border border-black w-16 h-16 relative overflow-hidden">
            <div
              class="absolute bottom-0 left-0 w-full bg-yellow-500 text-white flex items-center justify-center"
              :style="{ height: `${industrialData.listas / (industrialData.listas + industrialData.faltantes) * 100}%` }">
              {{ Math.round(industrialData.listas / (industrialData.listas + industrialData.faltantes) * 100) }}%
            </div>
          </div>
        </div>
      </div>
      <h3>Completados: {{ industrialData.listas }}</h3>
      <h3>Faltantes: {{ industrialData.faltantes }}</h3>
    </div>

    <!-- Estado General -->
    <div class="flex flex-col items-center w-32 h-48 md:w-72 md:h-44 p-4 rounded-full bg-[#badbcc] mb-2 md:mb-4 text-center">
      <h2>ESTADO GENERAL</h2>
      <i class="fa-solid fa-chart-simple p-2"></i><br>
      <div class="flex items-center">
        <p class="mr-2">{{ Math.round(info.alEvaluados / info.alTotal * 100) }}%</p>
        <div class="border border-black w-20 md:w-24 h-4 relative">
          <div
            class="h-full bg-red-500"
            :style="{ width: `${info.alEvaluados / info.alTotal * 100}%` }"
          ></div>
        </div>
      </div><br>
      <p>Completados {{ info.alEvaluados }} de {{ info.alTotal }}</p>
      <p>{{ info.alTotal - info.alEvaluados }} Faltantes</p>
    </div>
  </div>
</div>


</template>



