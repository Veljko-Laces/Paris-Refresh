<template>
    <Navbar />
    <h1 class="uppercase text-[#5f259f] text-4xl xl:mt-35 lg:mt-35 md:mt-35 mt-30 px-4 md:px-8 lg:px-20 xl:px-8">équipements et activités</h1>

    <div class="flex items-center mt-8 gap-2 flex flex-wrap px-4 md:px-8 lg:px-20 xl:px-8">
        <button @click="filters.isFree = !filters.isFree" :class="filters.isFree ? 'Active' : 'IsNotActive'">Équipements gratuits</button>
        <button @click="filters.isOpen = !filters.isOpen" :class="filters.isOpen ? 'Active' : 'IsNotActive'">Équipements ouverts</button>
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
        <div v-if="filteredEquipements.length === 0" class="mb-[40%] w-[800px] lg:w-[800px] md:w-fit w-fit flex flex-col gap-3">
            <b>Aucuns équipements et activités ne correspondent à ce filtre.</b>
            <p>Nous n'avons pas réussi à trouver d'équipements et d'activités qui correspondent à vos critères.</p>
            <button @click="resetFilters" class="rounded-md py-2 px-4 font-bold border border-grey-200 md:w-fit hover:border-[2px]" >Effacer la séléction</button>
        </div>

        <div v-for="equipement in filteredEquipements" :key="equipement.identifiant"
            class="bg-[#e7e5ff] h-fit w-full lg:w-[450px] 2xl:w-[450px] md:w-[400px] rounded-xl">

            <div class="bg-[#5f259f] px-4 py-4 text-white flex flex-col rounded-t-xl">
                <p class="uppercase font-semibold">{{ equipement.nom }}</p>
                <p class="font-medium">{{ equipement.type }}</p>
            </div>

            <div class="px-4 py-4 flex flex-col gap-1">
                <p><span class="text-[#5f259f] font-semibold">Adresse</span> : {{ equipement.adresse }}</p>
                <p><span class="text-[#5f259f] font-semibold">Code postale</span> : {{ equipement.arrondissement }}</p>
            </div>

            <div class="px-4 py-4 flex flex-row justify-between">
                <p><span class="text-[#5f259f] font-semibold">Payant</span> : {{ equipement.payant || "—" }}</p>
                <p><span class="text-[#5f259f] font-semibold">Ouvert</span> : {{ equipement.statut_ouverture || "—" }}
                </p>
            </div>

            <div class="px-4">
                <hr>
            </div>

            <div class="px-4 py-4">
                <p class="text-[#5f259f] font-semibold underline">Horaires :</p>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Lundi : </p>
                    <p>{{ equipement.horaires_lundi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Mardi : </p>
                    <p>{{ equipement.horaires_mardi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Mercredi : </p>
                    <p>{{ equipement.horaires_mercredi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Jeudi : </p>
                    <p>{{ equipement.horaires_jeudi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Vendredi : </p>
                    <p>{{ equipement.horaires_vendredi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Samedi : </p>
                    <p>{{ equipement.horaires_samedi || "—" }}</p>
                </div>
                <div class="flex flex-row justify-between items-center">
                    <p class="text-[#5f259f] font-semibold">Dimanche : </p>
                    <p>{{ equipement.horaires_dimanche || "—" }}</p>
                </div>
            </div>

        </div>

    </div>
    <Footer />

</template>

<script setup>
const { data: equipements } = await useFetch('https://opendata.paris.fr/api/explore/v2.1/catalog/datasets/ilots-de-fraicheur-equipements-activites/records?limit=100')

const filters = reactive({
    isFree: false,
    isOpen: false,
    selectedArrondissement: ''
})

const arrondissements = [
    { value: '', label: 'Tous les arrondissements' },
    { value: '75004', label: '4ème arrondissement' },
    { value: '75006', label: '6ème arrondissement' },
    { value: '75011', label: '11ème arrondissement' },
    { value: '75012', label: '12ème arrondissement' },
    { value: '75013', label: '13ème arrondissement' },
    { value: '75015', label: '15ème arrondissement' },
    { value: '75016', label: '16ème arrondissement' },
    { value: '75019', label: '19ème arrondissement' },
    { value: '75020', label: '20ème arrondissement' }
]

const filteredEquipements = computed(() => {
    return equipements.value.results.filter(equipement => {
        if (filters.isFree && equipement.payant !== "Non") return false;
        if (filters.isOpen && equipement.statut_ouverture !== "Ouvert") return false;
        
        if (filters.selectedArrondissement && equipement.arrondissement !== filters.selectedArrondissement) {
            return false;
        }
        
        return true;
    });
})

const resetFilters = () => {
    filters.isFree = false
    filters.isOpen = false
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