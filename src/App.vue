<script setup>
import { computed, ref, watch } from "vue";
import nethmiOne from "./assets/nethmi-1.jpeg";
import nethmiTwo from "./assets/nethmi-2.jpeg";
import nethmiThree from "./assets/nethmi-3.jpeg";
import nethmiFour from "./assets/nethmi-4.jpeg";
import surpriseAudioSource from "./assets/surprise-audio.mp3";

const candidateName = "Rathnayake Mudiyanselage Nethmi Navodya";
const currentScreen = ref(0);
const surpriseAudio = ref(null);
const screens = ["notice", "profile", "status", "reveal"];
const photos = [
  {
    src: nethmiOne,
    alt: `${candidateName} smiling`,
  },
  {
    src: nethmiTwo,
    alt: `${candidateName} wearing clear glasses`,
  },
  {
    src: nethmiThree,
    alt: `${candidateName} portrait`,
  },
  {
    src: nethmiFour,
    alt: `${candidateName} wearing a cap`,
  },
];

const isReveal = computed(() => currentScreen.value === screens.length - 1);

function playSurpriseAudio() {
  if (!surpriseAudio.value) {
    return;
  }

  surpriseAudio.value.currentTime = 0;
  surpriseAudio.value.play().catch(() => {});
}

function nextScreen() {
  const next = Math.min(currentScreen.value + 1, screens.length - 1);

  if (next === screens.length - 1) {
    playSurpriseAudio();
  }

  currentScreen.value = next;
}

function restart() {
  if (surpriseAudio.value) {
    surpriseAudio.value.pause();
    surpriseAudio.value.currentTime = 0;
  }

  currentScreen.value = 0;
}

watch(
  isReveal,
  (revealed) => {
    document.title = revealed
      ? "Happy Birthday, Nethmi!"
      : "Academic Advancement Scholarship Portal";
  },
  { immediate: true },
);
</script>

<template>
  <main class="app-shell">
    <section class="portal">
      <header class="topbar">
        <div class="brand">
          <span class="brand-mark">AA</span>
          <div>
            <p class="kicker">Academic Advancement Board</p>
            <h1>Scholarship Candidate Portal</h1>
          </div>
        </div>
        <span class="secure-pill">Secure Notice</span>
      </header>

      <div class="progress" aria-label="Application progress">
        <span
          v-for="(_, index) in screens"
          :key="index"
          class="progress-step"
          :class="{ active: index <= currentScreen }"
        ></span>
      </div>

      <audio
        ref="surpriseAudio"
        class="surprise-audio"
        :src="surpriseAudioSource"
        preload="auto"
      ></audio>

      <Transition name="screen-fade" mode="out-in">
        <section v-if="currentScreen === 0" key="notice" class="screen active">
          <div class="notice-grid">
            <div class="notice-copy">
              <p class="eyebrow">Priority review required</p>
              <h2>Degree Support Scholarship Offer</h2>
              <p>
                A confidential scholarship notice has been prepared for your
                current degree pathway. Please continue to verify candidate
                details and view the award decision.
              </p>
              <button class="primary" @click="nextScreen">View Notice</button>
            </div>
            <aside class="document-card">
              <div class="doc-seal">AAB</div>
              <p>Candidate Decision Packet</p>
              <span>Ref: AAB-2026-BD</span>
              <div class="doc-lines">
                <i></i><i></i><i></i><i></i>
              </div>
            </aside>
          </div>
        </section>

        <section v-else-if="currentScreen === 1" key="profile" class="screen active">
          <p class="eyebrow">Candidate verification</p>
          <h2>Confirm applicant profile</h2>
          <div class="profile-card">
            <div>
              <span>Applicant</span>
              <strong>{{ candidateName }}</strong>
            </div>
            <div>
              <span>Program</span>
              <strong>Undergraduate Degree</strong>
            </div>
            <div>
              <span>Review Type</span>
              <strong>Special Merit Award</strong>
            </div>
          </div>
          <p class="hint">
            This award requires final acknowledgement before the decision can
            be released.
          </p>
          <button class="primary" @click="nextScreen">Confirm Details</button>
        </section>

        <section v-else-if="currentScreen === 2" key="status" class="screen active">
          <p class="eyebrow">Final review</p>
          <h2>Scholarship status</h2>
          <div class="status-box">
            <span class="status-dot"></span>
            <div>
              <strong>Decision ready</strong>
              <p>
                Your result has been approved and is waiting behind the final
                acknowledgement.
              </p>
            </div>
          </div>
          <div class="checklist">
            <span>Academic potential verified</span>
            <span>Best-friend hype committee consulted</span>
            <span>Celebration package prepared</span>
          </div>
          <button class="primary" @click="nextScreen">Release Decision</button>
        </section>

        <section v-else key="reveal" class="screen reveal-screen active">
          <div class="confetti" aria-hidden="true">
            <i v-for="piece in 12" :key="piece"></i>
          </div>
          <div class="reveal-layout">
            <div class="reveal-message">
              <p class="eyebrow">Official decision</p>
              <h2>
                <span class="nowrap">Congratulations,</span>
                {{ candidateName }}!
              </h2>
              <p class="reveal-copy">
                You have been awarded a lifetime scholarship of love, laughter,
                chaos, and unlimited birthday wishes from your friends.
              </p>
              <div class="birthday-card">
                <span>Surprise!</span>
                <strong>Happy Birthday</strong>
                <p>
                  We hope your new year is full of beautiful moments, brave
                  dreams, and every success your degree is leading you toward.
                </p>
              </div>
              <p class="from-line">
                From Kula baba &amp; DBunny Senanayake - sponsored by Keheralla
                Sulage
              </p>
              <button class="secondary" @click="restart">Open Again</button>
            </div>

            <div class="photo-wall" aria-label="Birthday photos">
              <figure v-for="photo in photos" :key="photo.src" class="photo-card">
                <img :src="photo.src" :alt="photo.alt" />
              </figure>
            </div>
          </div>
        </section>
      </Transition>
    </section>
  </main>
</template>
