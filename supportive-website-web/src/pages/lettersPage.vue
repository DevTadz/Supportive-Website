<template>
    <q-page class="row items-center justify-evenly" style="background-color: #F8E8C7;">
        <q-btn style="height: 100px; width: 100px;" @click="generateLetter">
            <q-icon name="email" size="xl"></q-icon>
        </q-btn>
    </q-page>

    <q-dialog v-model="letterDialog">
      <q-card>
        <q-card-section>
          <div class="text-h6">{{selectedLetter?.title}}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
            {{selectedLetter?.content}}
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup></q-btn>
        </q-card-actions>
      </q-card>
    </q-dialog>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Letter {
    title: string;
    content: string;
}

const letterDialog = ref<boolean>(false);

const letters = ref<Letter[]>([
    {
        title: "Hard times",
        content: "I know things feel like a lot right now. Like everything is piling up at once and it’s hard to catch your breath. I just want you to know you don’t have to carry it all perfectly. You’re allowed to feel tired. You’re allowed to not have everything figured out. None of that changes how strong or important you are."
    },
    {
        title: "♡",
        content: "I care alot about you, always remember that :)"
    },
])

const selectedLetter = ref<Letter>();

function generateLetter() {
    letterDialog.value = true
    const letterIndex = Math.floor(Math.random() * letters.value.length);
    selectedLetter.value = letters.value[letterIndex];
}
</script>