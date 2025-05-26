<script setup>
import { ref } from 'vue';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
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
  <Card>
    <CardHeader>
      <CardTitle>Voucher Game</CardTitle>
      <CardDescription>Pilih game favorit Anda untuk melakukan top up voucher</CardDescription>
    </CardHeader>
    <ScrollArea class="h-full">
      <CardContent class="space-y-6">
        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-4">
          <Card
            v-for="game in games"
            :key="game.id"
            class="group cursor-pointer p-0"
            :class="selectedGame === game.id ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
            @click="selectGame(game.code)"
          >
            <CardContent class="p-4">
              <div class="relative overflow-hidden rounded-lg mb-4">
                <NuxtImg
                  :src="game.image"
                  :alt="game.name"
                  class="w-full h-48 object-cover transition-transform duration-300 group-hover:scale-110"
                  loading="lazy"
                />
              </div>
              <div class="text-center">
                <div class="font-bold">
                  {{ game.name }}
                </div>
                <div class="text-sm">
                  {{ game.developer }}
                </div>
              </div>
            </CardContent>
          </Card>
        </div>
      </CardContent>
    </ScrollArea>
  </Card>
</template>
