<template>
  <div class="grid-cols-1 p-20 bg-red-100 h-screen">
    <form @submit.prevent="ajouterTache">
      <div class="mb-4">
        <label for="nouvelleTache" class="text-lg font-medium text-gray-700">Nouvelle Tâche :</label>
        <input v-model="nouvelleTache" type="text" id="nouvelleTache" name="nouvelleTache"
          class="mt-1 p-2 border rounded-md w-full" placeholder="Entrez votre nouvelle tâche" />
      </div>
      <button type="submit" class=" bg-white	text-black p-2 rounded-md">Ajouter</button>
    </form>

    <ul class="mt-4">
      <h2 class="underline decoration-1 text-lg pb-2">Listes des tâches</h2>
      <li v-for="(tache, index) in taches" :key="index" class="flex items-center mb-2 ">
        <input type="checkbox" v-model="tache.terminee" class="mr-2" />
        <span :class="{ 'line-through': tache.terminee }">{{ tache.texte }}</span>
        <button @click="demanderConfirmation(index)" class="ml-2 text-red-500">Supprimer</button>
      </li>
    </ul>

    <!-- Modal de confirmation -->
    <div v-if="confirmationVisible" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
      <div class="bg-white p-8 rounded-md">
        <p class="text-lg font-medium text-gray-700 mb-4">Êtes-vous sûr de vouloir supprimer cette tâche ?</p>
        <div class="flex justify-end">
          <button @click="confirmerSuppression" class="bg-red-500 text-white p-2 rounded-md mr-2">Oui</button>
          <button @click="annulerSuppression" class="bg-gray-300 p-2 rounded-md">Annuler</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nouvelleTache: '',
      taches: [],
      tacheASupprimer: null,
      confirmationVisible: false
    };
  },
  mounted() {
    // Charger les tâches sauvegardées
    const tachesSauvegardees = localStorage.getItem('taches');
    if (tachesSauvegardees) {
      this.taches = JSON.parse(tachesSauvegardees);
    }
  },
  watch: {
    // Surveiller les changements dans les tâches et les sauvegarder
    taches: {
      handler() {
        localStorage.setItem('taches', JSON.stringify(this.taches));
      },
      deep: true
    }
  },
  methods: {
    ajouterTache() {
      if (this.nouvelleTache.trim() !== '') {
        this.taches.push({
          texte: this.nouvelleTache,
          terminee: false
        });
        this.nouvelleTache = '';
      }
    },
    demanderConfirmation(index) {
      this.tacheASupprimer = index;
      this.confirmationVisible = true;
    },
    confirmerSuppression() {
      if (this.tacheASupprimer !== null) {
        this.taches.splice(this.tacheASupprimer, 1);
        this.tacheASupprimer = null;
        this.confirmationVisible = false;
      }
    },
    annulerSuppression() {
      this.tacheASupprimer = null;
      this.confirmationVisible = false;
    }
  }
};
</script>

<style scoped>
/* Ajoutez vos styles Tailwind CSS personnalisés ici si nécessaire */
</style>