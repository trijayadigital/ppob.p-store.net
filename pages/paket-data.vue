<script setup>
import { ref, watch } from 'vue';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import ProductSelector from '@/components/ProductSelector.vue';

// Menggunakan layout products
definePageMeta({
  layout: 'products',
});

const phoneNumber = ref('');
const operator = ref(null);
const products = ref([]);
const selectedProduct = ref('');

// Inject selectedProduct dari layout
const layoutSelectedProduct = inject('selectedProduct');

const checkOperator = async (prefix) => {
  try {
    const response = await fetch(`https://tripay.id/process/prefixproduct?category_id=2&prefix=${prefix}`);
    const data = await response.json();

    if (data.operator && data.operator.length > 0) {
      operator.value = data.operator[0];
      products.value = data.produk || [];
    }
  } catch (error) {
    console.error('Error fetching operator:', error);
  }
};

watch(phoneNumber, (newValue) => {
  const cleanNumber = newValue.replace(/\D/g, '');
  phoneNumber.value = cleanNumber;

  if (cleanNumber.length >= 4) {
    const prefix = cleanNumber.substring(0, 4);
    checkOperator(prefix);
  } else {
    operator.value = null;
    products.value = [];
  }
});

watch(selectedProduct, (newValue) => {
  const product = products.value.find((p) => p.product_id === newValue);
  if (layoutSelectedProduct) {
    layoutSelectedProduct.value = product;
  }
});
</script>

<template>
  <Card>
    <CardHeader>
      <CardTitle>Paket Data</CardTitle>
    </CardHeader>
    <CardContent class="space-y-4">
      <div>
        <label class="block text-sm text-gray-600 mb-2">Nomor Telepon</label>
        <input
          v-model="phoneNumber"
          type="text"
          placeholder="Silakan Masukkan No. Handphone-mu"
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent"
          maxlength="13"
        />
        <div v-if="operator" class="mt-2 text-sm text-gray-600">
          <p class="font-medium">{{ operator.product_name }}</p>
        </div>
      </div>

      <ProductSelector v-model="selectedProduct" :products="products" :columns="3" :show-description="true">
        <template #empty-message> Masukkan nomor telepon untuk melihat paket data yang tersedia </template>
      </ProductSelector>
    </CardContent>
  </Card>
</template>
