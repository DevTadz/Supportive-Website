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
    <q-card dark>
      <q-card-section>
        <div style="width: 100%;" class="row justify-center">
          <img src="../assets/nana_rose.jpg" style="height: 200px; width: auto;">
        </div>
        <div class="text-h6">{{selectedSong?.title}}</div>
        <div>{{selectedSong?.author}}</div>

            <q-linear-progress
              :value="progress"
              rounded
              color="secondary"
              style="background-color: white;"
              class="q-mt-sm"
            ></q-linear-progress>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <div class="row justify-between">
          <q-btn icon="arrow_back_ios" class="bg-white mediaPLayerButton" rounded></q-btn>
          <q-btn @click="togglePlay" :icon="playButtonicon" class="bg-white mediaPLayerButton" rounded></q-btn>
          <q-btn icon="arrow_forward_ios" class="bg-white mediaPLayerButton" rounded></q-btn>
        </div>
      </q-card-section>

      <q-card-actions align="right">
       
      </q-card-actions>
    </q-card>
  </q-dialog>
  <audio ref="audioPlayer" :src="songFile" @timeupdate="updateProgress"></audio>
</template>


<script setup lang="ts">
import songFile from '../assets/music/nana.mp3'
import { ref, onMounted, onBeforeUnmount} from 'vue';
// import { useRoute } from 'vue-router';
import { useRouter } from 'vue-router';

const mediaPlayer = ref<boolean>(false)
const router = useRouter();

const audioPlayer = ref<HTMLAudioElement | null>(null)

const progress = ref(0);

const updateProgress = () => {
  if (!audioPlayer.value) return

  const current = audioPlayer.value.currentTime
  const duration = audioPlayer.value.duration

  if (duration > 0) {
    progress.value = (current / duration)
  }
}

onMounted(() => {
  audioPlayer.value?.addEventListener('timeupdate', updateProgress)
})

onBeforeUnmount(() => {
  audioPlayer.value?.removeEventListener('timeupdate', updateProgress)
})

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

const playIcon = ref<string>("play_arrow");
const pauseIcon = ref<string>("pause");
const playButtonicon = ref<string>("play_arrow");
const isPlayingSong = ref<boolean>(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}

async function changePage(route: string) {
  await router.push(route);
}

async function togglePlay() {
  if(!isPlayingSong.value) {
    isPlayingSong.value = true;
    playButtonicon.value = pauseIcon.value;
    await audioPlayer.value?.play()
  }
  else {
    isPlayingSong.value = false;
    playButtonicon.value = playIcon.value;
    audioPlayer.value?.pause()
  }
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

.mediaPLayerButton {
  color: black;
  width: 40px;
  height: 40px;
}
</style>
