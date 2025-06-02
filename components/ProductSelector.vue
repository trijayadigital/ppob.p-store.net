<script setup>
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group';
import { Label } from '@/components/ui/label';

// Props yang diterima komponen
const props = defineProps({
  products: {
    type: Array,
    default: () => [],
  },
  modelValue: {
    type: String,
    default: '',
  },
  columns: {
    type: Number,
    default: 4,
  },
  showDescription: {
    type: Boolean,
    default: true,
  },
  priceField: {
    type: String,
    default: 'price_personal',
  },
  nameField: {
    type: String,
    default: 'product_name',
  },
  descField: {
    type: String,
    default: 'desc',
  },
  idField: {
    type: String,
    default: 'product_id',
  },
});

// Emit untuk v-model
const emit = defineEmits(['update:modelValue']);

// Computed untuk grid classes
const gridClasses = computed(() => {
  const columnMap = {
    2: 'grid-cols-2',
    3: 'grid-cols-3',
    4: 'grid-cols-4',
    5: 'grid-cols-5',
    6: 'grid-cols-6',
  };
  return `grid ${columnMap[props.columns] || 'grid-cols-4'} gap-2`;
});

// Handler untuk update model value
const updateValue = (value) => {
  emit('update:modelValue', value);
};
</script>

<template>
  <div v-if="products.length > 0" class="space-y-4">
    <RadioGroup :model-value="modelValue" @update:model-value="updateValue" :class="gridClasses">
      <div v-for="product in products" :key="product[idField]" class="relative">
        <RadioGroupItem :value="product[idField]" class="peer sr-only" :id="product[idField]" />
        <Label
          :for="product[idField]"
          class="flex justify-between p-4 border rounded-lg cursor-pointer transition-all duration-200 hover:shadow-md peer-data-[state=checked]:border-primary peer-data-[state=checked]:bg-primary/10 peer-data-[state=checked]:shadow-lg"
        >
          <div class="flex flex-col gap-y-1">
            <span class="text-sm font-bold">{{ product[nameField] }}</span>
            <span v-if="showDescription && product[descField]" class="text-xs text-gray-600">{{ product[descField] }}</span>
          </div>
          <span class="text-primary text-base font-bold"> Rp {{ Number(product[priceField]).toLocaleString('id-ID') }} </span>
        </Label>
      </div>
    </RadioGroup>
  </div>

  <div v-else class="flex flex-col items-center justify-center h-full p-6 gap-y-4">
    <img src="/empty.png" alt="No Products" class="w-48" />
    <p class="text-gray-400 font-medium">
      <slot name="empty-message"> Tidak ada produk yang tersedia </slot>
    </p>
  </div>
</template>
