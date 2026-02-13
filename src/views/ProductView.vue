<template>
    <v-main>
        <v-container fluid>
            <v-row>
                <v-col cols="12">
                    
                    <v-card v-for="p in product">
                        <v-card-title>{{ p.name }}</v-card-title>
                        <v-card-text>
                            {{ p.description }} {{ p.price }}
                        </v-card-text>
                        <v-card-actions>
                            <v-btn color="primary" :to="`/product/${p.id}`">เหลือ {{ p.stock }}</v-btn>
                        </v-card-actions>
                    </v-card>

                </v-col>
            </v-row>
        </v-container>
    </v-main>
</template>

<script setup>
    import { onMounted, ref } from 'vue';

    const product = ref([])

    onMounted(async () => {
        const response = await fetch("http://localhost:3000/product")
        const data = await response.json()
        product.value = data
    })


</script>