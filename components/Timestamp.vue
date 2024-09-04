<template>
  <div class="timestamp-container">
    <p class="label">Текущая временная метка:</p>
    <p class="timestamp">{{ timestamp }}</p>
  </div>
</template>

<script setup lang="ts">
const timestamp = ref<string>('');
let intervalId: NodeJS.Timeout;

const { data: serverTimestamp } = await useAsyncData('timestamp', async () => {
  const response = await $fetch<{ timestamp: string }>('/api/timestamp');
  return response.timestamp;
});

timestamp.value = serverTimestamp.value || '';

const updateTimestamp = () => {
  timestamp.value = new Date().toISOString();
};

onMounted(() => {
  updateTimestamp();
  intervalId = setInterval(updateTimestamp, 10000);
});

onUnmounted(() => {
  clearInterval(intervalId);
});
</script>

<style scoped lang="scss">
.timestamp-container {
  background-color: #f0f4f8;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: 20px auto;

  .label {
    font-size: 1.2em;
    color: #333;
    margin-bottom: 10px;
  }

  .timestamp {
    font-size: 1.4em;
    font-weight: bold;
    color: #007acc;
  }
}
</style>
