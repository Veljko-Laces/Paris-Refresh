<template>
    <Navbar />
    <h1 class="uppercase text-[#5f259f] text-4xl xl:mt-35 lg:mt-35 md:mt-35 mt-30 px-4 md:px-8 lg:px-20 xl:px-8">fontaines à boire</h1>

    <div class="flex items-center mt-8 gap-2 flex flex-wrap px-4 md:px-8 lg:px-20 xl:px-8">
        <button @click="filters.isDisponible = !filters.isDisponible" :class="filters.isDisponible ? 'Active' : 'IsNotActive'">Fontaines disponibles</button>
        <label class="w-fit rounded-4xl border border-[#5f259f] px-2 py-2 text-[#5f259f] hover:font-bold hover:border-[2px]">
            Arrondissement :
            <select v-model="filters.selectedArrondissement">
                <option v-for="arrondissement in arrondissements" :key="arrondissement.value" :value="arrondissement.value">
                    {{ arrondissement.label }}
                </option>
            </select>
        </label>
    </div>

    <div class="px-4 grid grid-cols-1 gap-4 md:grid-cols-2 md:px-8 lg:grid-cols-2 lg:px-20 xl:grid-cols-3 xl:px-8 mt-10 mb-10">
        <div v-if="filteredFontaines.length === 0 " class="mb-[40%] w-[800px] lg:w-[800px] md:w-fit w-fit flex flex-col gap-3">
            <b>Aucune fontaine ne correspond à ce filtre.</b>
            <p>Nous n'avons pas réussi à trouver de fontaines qui correspondent à vos critères.</p>
            <button @click="resetFilters" class="rounded-md py-2 px-4 font-bold border border-grey-200 md:w-fit hover:border-[2px]" >Effacer la séléction</button>
        </div>

        <div v-for="fontaine in filteredFontaines" :key="fontaine.gid"
            class="bg-[#e7e5ff] h-fit w-full lg:w-[450px] 2xl:w-[450px] md:w-[400px] rounded-xl">

            <div class="bg-[#5f259f] px-4 py-4 text-white flex flex-col rounded-t-xl">
                <p class="uppercase font-semibold">{{ fontaine.modele }}</p>
                <p class="font-medium">Fontaines à boire</p>
            </div>

            <div class="px-4 py-2 flex flex-col gap-1">
                <p><span class="text-[#5f259f] font-semibold">Arrondissement</span> : {{ fontaine.commune }}</p>
                <p><span class="text-[#5f259f] font-semibold">Emplacement</span> : {{ fontaine.voie }}</p>
            </div>

            <div class="px-4 py-4 flex flex-col gap-1">
                <p><span class="text-[#5f259f] font-semibold">Disponible</span> : {{ fontaine.dispo || "—" }}</p>
                <p><span class="text-[#5f259f] font-semibold">Motif de l'indisponibilité</span> : {{ fontaine.motif_ind|| "—" }}</p>
            </div>

        </div>

    </div>

    <Footer />
</template>

<script setup>
const { data: fontaines } = await useFetch('https://parisdata.opendatasoft.com/api/explore/v2.1/catalog/datasets/fontaines-a-boire/records?limit=100')

const filters = reactive({
    isDisponible: false,
    selectedArrondissement: '' 
})

const arrondissements = [
    { value: '', label: 'Tous les arrondissements' },
    { value: 'PARIS 5EME ARRONDISSEMENT', label: '5ème arrondissement' },
    { value: 'PARIS 7EME ARRONDISSEMENT', label: '7ème arrondissement' },
    { value: 'PARIS 11EME ARRONDISSEMENT', label: '11ème arrondissement' },
    { value: 'PARIS 12EME ARRONDISSEMENT', label: '12ème arrondissement' },
    { value: 'PARIS 13EME ARRONDISSEMENT', label: '13ème arrondissement' },
    { value: 'PARIS 14EME ARRONDISSEMENT', label: '14ème arrondissement' },
    { value: 'PARIS 15EME ARRONDISSEMENT', label: '15ème arrondissement' },
    { value: 'PARIS 18EME ARRONDISSEMENT', label: '18ème arrondissement' },
    { value: 'PARIS 19EME ARRONDISSEMENT', label: '19ème arrondissement' }
]

const filteredFontaines = computed(() => {
    return fontaines.value.results.filter(fontaine => {
        if (filters.isDisponible && fontaine.dispo !== "OUI") return false;
        
        if (filters.selectedArrondissement && fontaine.commune !== filters.selectedArrondissement) {
            return false;
        }
        
        return true;
    });
})

const resetFilters = () => {
    filters.isDisponible = false
    filters.selectedArrondissement = ''
}
</script>

<style scoped>

.Active {
    display: flex;                      
    width: fit-content;                 
    padding: 0.5rem 0.5rem;              
    border-radius: 2rem;  
    color: white;
    background: #5f259f;
    cursor: pointer;
    font-weight: bold;
}

.IsNotActive {
    display: flex;                      
    width: fit-content;                 
    padding: 0.5rem 0.5rem;              
    border-radius: 2rem;                 
    border: 1px solid #5f259f;   
    color: #5f259f; 
    cursor: pointer;  
}

.IsNotActive:hover {
    color: #5f259f;
    border: 2px solid #5f259f ;
    background-color: white ;
    font-weight: bold;
}
</style>