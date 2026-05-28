<template>
  <q-layout view="lHh Lpr lFf">
      <q-toolbar style="background-color: #F8E8C7;">
        <img src="../assets/logo.svg" @click="toggleLeftDrawer" class="logo">
        <q-toolbar-title class="nunito"> Support App </q-toolbar-title>
        <q-btn round color="primary" icon="music_note" @click="mediaPlayer = true"></q-btn>
      </q-toolbar>

    <q-drawer v-model="leftDrawerOpen" class="bg-primary" dark>
      <q-list>
        <div class="row justify-between">
          <q-item-label header> Menu </q-item-label>
          <q-btn round icon="close" style="height: 30px; width: 30px;" flat></q-btn>
        </div>

        <q-btn flat class="menuButton q-mt-md nunito" v-for="link in linksList" :key="link.title" @click="changePage(link.link)">
          <div class="row" style="width: 100%;">
            <q-icon :name="link.icon"></q-icon>
            <div class="q-pl-xl">{{ link.title }}</div>
          </div>
        </q-btn>

        <!-- <EssentialLink v-for="link in linksList" :key="link.title" v-bind="link" /> -->
      </q-list>
    </q-drawer>

    <q-page-container style="max-height: 100%;">
      <router-view/>
    </q-page-container>
  </q-layout>

  <q-dialog v-model="mediaPlayer">
    <q-card>
      <q-card-section>
        <div class="text-h6">{{selectedSong?.title}}</div>
        <div>{{selectedSong?.author}}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <audio controls>
          <source src="src\assets\music\nana.mp3" type="audio/mpeg">
          Your browser does not support the audio element.
        </audio>
        <!-- <q-btn @click="Skip">Skip</q-btn> -->
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Close" color="primary" v-close-popup></q-btn>
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup lang="ts">
import { ref } from 'vue';
// import { useRoute } from 'vue-router';
import { useRouter } from 'vue-router';

const mediaPlayer = ref<boolean>(false)
const router = useRouter();

interface Link {
  title: string;
  link: string;
  icon: string;
}

interface Song {
  rank: number;
  title: string;
  author: string;
  musicSource: string;
}

const linksList: Link[] = [
  {
    title: 'Home',
    icon: 'home',
    link: '/',
  },
  {
    title: 'Letters',
    icon: 'mail',
    link: '/letters',
  }
];



const songList = ref<Song[]>([
  {rank: 0, title: "Rose - Nana", author: "BLACK STONES", musicSource: "../assets/music/nana.mp3"}
]);

const selectedSong = ref<Song | undefined>(songList.value[0]);

const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}

async function changePage(route: string) {
  await router.push(route);
}

// function Skip() {
//   if(selectedSong.value == undefined)
//     return;

//   if(selectedSong.value.rank + 1 > songList.value.length) {
//     selectedSong.value = songList.value[0];
//   }
//   else {
//     selectedSong.value = songList.value[selectedSong.value.rank + 1];
//   }
// }
</script>

<style lang="css">
.logo {
  cursor: pointer;
  height: 70px; 
  max-width: 70px;
  transition: 0.3s;
}

.logo:hover {
  max-width: 80px;
  height: 80px;
}

.menuButton {
  height: 50px;
  width: 100%;
}
</style>
