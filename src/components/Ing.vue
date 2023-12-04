<script setup>
import { onMounted, ref } from 'vue';
const info =ref('')

async function fetchData() {
try {
const response = await fetch('https://sitmotul.com.mx/api/statusEvalIng');
const data = await response.json();
info.value=data 

console.log(info.value)
}catch (error) {
 console.error('Error al obtener datos:', error);
  }
}

onMounted(() => {
fetchData();
})
</script>