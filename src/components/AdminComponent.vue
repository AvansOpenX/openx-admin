<template>
  <div class="p-2 pb-5 mt-2 d-flex flex-column gap-4">
    <section>
      <h2>BLE Pincode</h2>
      <div class="d-flex flex-row gap-2 align-items-center">
        <input type="text" class="rounded form-control w-100" maxlength="6" v-model="settings.pin" />
      </div>
    </section>
    <section>
      <h1>
        Plant instellingen
      </h1>
      <hr>
      <!-- Foreach plant -->
      <PlantSettings v-for="(plant, i) in settings.plants" :key="i" :index="i" :value="plant" />
      <!-- End foreach -->
    </section>
    <section>
      <h1>Algemene instellingen</h1>
      <hr>
      <div class="d-flex flex-column gap-2 pb-2 pt-2">
        <div>
          <h2>Meet interval</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" class="rounded form-control" v-model="settings.mInterval" />
            <p class="mb-0">Seconden</p>
          </div>
        </div>
        <div>
          <h2>Spelduur</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" class="rounded form-control" v-model="settings.gameDuration" />
            <p class="mb-0">Seconden</p>
          </div>
        </div>
        <div>
          <h2>Reservoir kraan</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" class="rounded form-control" v-model="settings.rValveFlow" />
            <p class="mb-0">Procent</p>
          </div>
        </div>
        <div>
          <h2>Maximale batterijduur</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" class="rounded form-control" v-model="settings.battDrain" />
            <p class="mb-0">Minuten</p>
          </div>
        </div>
        <div>
          <h2>Minimale aantal zonuren</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" max="24" class="rounded form-control" v-model="settings.sunHours" />
            <p class="mb-0">Uur</p>
          </div>
        </div>
        <div>
          <h2>Actieve periode</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" min="0" max="24" class="rounded form-control" v-model="settings.activeStart" />
            <input type="number" min="0" max="24" class="rounded form-control" v-model="settings.activeEnd" />
          </div>
        </div>
        <div>
          <h2>Data verzend interval</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="number" class="rounded form-control" v-model="settings.tInterval" />
            <p class="mb-0">Minuten</p>
          </div>
        </div>
        
      </div>
    </section>
    <section>
      <h1>WiFi instellingen*</h1>
      <hr>
      <div class="d-flex flex-column gap-2 pb-2 pt-2">
        <div>
          <h2>SSID</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="text" class="rounded form-control w-100" maxlength="32" v-model="settings.ssid" />
          </div>
        </div>
        <div>
          <h2>Wachtwoord</h2>
          <div class="d-flex flex-row gap-2 align-items-center">
            <input type="text" class="rounded form-control w-100" maxlength="32" v-model="settings.pass" />
          </div>
        </div>
      </div>
      <p class="mb-0 mt-1 disclaimer-text">* Systeem wordt automatish herstart na het doorvoeren van wijzigingen in deze categorie</p>
    </section>
  </div>
  <button id="saveButton" type="button" :class="{ show: true }" class="btn text-white rounded-circle shadow" @click="save">
    <i class="fa-solid fa-check"></i>
  </button>
</template>

<script>
  import { mapWritableState } from 'pinia'
  import { useDataStore } from '@/store/dataStore';
  import PlantSettings from '@/components/PlantSettings.vue';
  import { writeSettings } from '@/espConnection';

  export default {
    components: {
      PlantSettings,
    },

    computed: {
      ...mapWritableState(useDataStore, ['settings']),
    },

    methods: {
      async save() {
        try {
          const dataStore = useDataStore();
          if (dataStore.settings.pin.length == 6) await writeSettings();
          else alert("Vul een geldige BLE pincode in");
        } catch (e) {
          alert(e.message);
        }
      },
    },
  };
</script>
