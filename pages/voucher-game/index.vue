<script setup>
import { ref } from 'vue';
import { ScrollArea } from '@/components/ui/scroll-area';
import data from '@/data.json';
import { useRouter } from 'nuxt/app';

// Menggunakan layout products
definePageMeta({
  layout: 'products',
});

const router = useRouter();

// Data dari JSON file
const games = ref(data.games);

const selectGame = (gameCode) => {
  router.push(`/voucher-game/${gameCode}`);
};
</script>

<template>
  <!-- Main Card Container -->
  <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
    <!-- Card Header -->
    <ScrollArea class="h-full">
      <div class="flex flex-col space-y-1.5 p-6">
        <h3 class="text-2xl font-semibold leading-none tracking-tight">Voucher Game</h3>
        <p class="text-sm text-muted-foreground">Pilih game favorit Anda untuk melakukan top up voucher</p>
      </div>

      <!-- Card Content -->
      <div class="p-6 pt-0 space-y-6">
        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-4">
          <!-- Game Card Items -->
          <div
            v-for="game in games"
            :key="game.id"
            class="group cursor-pointer rounded-lg border bg-card text-card-foreground hover:ring-2 hover:ring-primary hover:bg-primary/10 shadow-sm transition-all duration-300 hover:shadow-lg"
            :class="selectedGame === game.id ? 'ring-2 ring-primary bg-primary/5' : ''"
            @click="selectGame(game.code)"
          >
            <!-- Game Card Content -->
            <div class="p-2">
              <div class="relative overflow-hidden rounded-lg mb-4">
                <NuxtImg :src="game.image" :alt="game.name" class="w-full h-48 object-cover transition-transform duration-300" loading="lazy" />
              </div>
              <div class="text-center">
                <div class="font-bold text-card-foreground">
                  {{ game.name }}
                </div>
                <div class="text-sm text-muted-foreground">
                  {{ game.developer }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </ScrollArea>
  </div>
</template>
