<script setup>
import { ref } from 'vue';

const lights = ref([
    { id: 'redLight', isOn: false, class: 'bg-danger' },
    { id: 'orangeLight', isOn: false, class: 'bg-warning' },
    { id: 'greenLight', isOn: false, class: 'bg-success' },
]);

const buttons = ref([
    { id: 'redButton', mode: 'manual', lightId: 'redLight', class: 'bg-danger', label: 'Red'},
    { id: 'orangeButton', mode: 'manual', lightId: 'orangeLight', class: 'bg-warning', label: 'Orange'},
    { id: 'greenButton', mode: 'manual', lightId: 'greenLight', class: 'bg-success', label: 'Green' },
    { id: 'autoButton', mode: 'auto', lightId: 'auto', class: 'bg-info', label: 'Auto'},
    { id: 'offButton', mode: 'off', lightId: 'off', class: 'bg-info', label: 'Off'},
]);

let autoInterval = null;
let blinkInterval = null;

function switchOn(light) {
    lights.value.forEach((lightItem) => {
        lightItem.isOn = (light === lightItem.id);
    });
}

function handleClick(button) {
    if (autoInterval) {
        clearInterval(autoInterval);
        autoInterval = null;
    }

    if (blinkInterval) {
        clearInterval(blinkInterval);
        blinkInterval = null;
    }
    
    switch (button.mode) {
        case "manual":
            switchOn(button.lightId);
            break;
        case "auto":
            handleAutoMode();
            break;
        case "off":
            handleOffMode();
            break;
        default:
            break;
    }
}

function handleAutoMode() {
    let currentLightIndex = 2; 
    const lightCycle = ['greenLight', 'orangeLight', 'redLight'];
    
    autoInterval = setInterval(() => {
        switchOn(lightCycle[currentLightIndex]);
        currentLightIndex = (currentLightIndex + 1) % lightCycle.length;
    }, 1000); 
}

function handleOffMode() {
    lights.value.forEach((lightItem) => {
        lightItem.isOn = false;
    });
      const orangeLight = lights.value.find(light => light.id === 'orangeLight');
    blinkInterval = setInterval(() => {
        if (orangeLight) {
            orangeLight.isOn = !orangeLight.isOn;
        }
    }, 1000);
}
</script>

<template>
    <div class="d-flex mt-5 justify-content-center">
        <div class="p-0 justify-content-center">
            <div class="d-flex flex-column mx-5 justify-content-center">
                <div :id="light.id" v-for="light in lights" :key="light.id"
                    :class="['text-center', 'my-1', light.isOn ? light.class : '', 'border', 'border-dark', 'rounded-circle']"
                    style="height: 146px; width: 146px;"></div>
            </div>
            <div class="mt-5 px-3 justify-content-between d-flex">
                <button :id="button.id" v-for="button in buttons" :key="button.id"
                    @click="handleClick(button)"
                    :class="['btn', button.class, 'mx-1']"
                    style="height: 30px; width: 60px;">
                    {{ button.label }}
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>
