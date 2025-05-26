<script setup>
// Halaman utama sementara dikosongkan
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-slate-100">
    <div class="text-center">
      <h1 class="text-4xl font-bold text-gray-800 mb-4">Selamat Datang</h1>
      <p class="text-gray-600 mb-8">Pilih layanan yang Anda butuhkan</p>

      <div class="grid grid-cols-2 md:grid-cols-3 gap-4 max-w-2xl">
        <NuxtLink to="/pulsa" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="lucide:smartphone" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Pulsa</span>
        </NuxtLink>

        <NuxtLink to="/paket-data" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="lucide:wifi" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Paket Data</span>
        </NuxtLink>

        <NuxtLink to="/voucher-game" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="lucide:gamepad" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Voucher Game</span>
        </NuxtLink>

        <NuxtLink to="/tagihan" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="heroicons:bolt-solid" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Tagihan</span>
        </NuxtLink>

        <NuxtLink to="/entertainment" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="lucide:music" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Entertainment</span>
        </NuxtLink>

        <NuxtLink to="/lainnya" class="p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <Icon name="lucide:more-horizontal" size="32" class="mx-auto mb-2 text-primary" />
          <span class="block font-semibold">Lainnya</span>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
