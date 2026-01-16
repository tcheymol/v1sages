<script setup>
import scope from '~/assets/scope.json';
import JSConfetti from 'js-confetti'



const persons = scope.data.items.filter(person => person?.picture?.name);
const findRandomPerson = () => persons[Math.floor(Math.random() * persons.length)];
const isModalOpen = ref(false);
const person = ref(null);
const ramdomPersons = ref([]);
const guessedPerson = ref([]);


function guessPerson(attemptedPerson) {
  if (attemptedPerson === person.value) {
    guessedPerson.value = attemptedPerson
    const jsConfetti = new JSConfetti()
    jsConfetti.addConfetti({
        emojis: ['🦄'],
      confettiRadius: 6,
      confettiNumber: 100,
    })
    isModalOpen.value = true;
    changePerson();
  };
}

function changePerson() {
  person.value = findRandomPerson();
  ramdomPersons.value = [person.value, findRandomPerson(), findRandomPerson(), findRandomPerson()].sort(() => Math.random() - 0.5);
}

onMounted(() => {
  changePerson();
});
</script>

<template>
  <div class="min-h-[calc(100vh-var(--ui-header-height))] bg-yellow-300 dark:bg-black">
    <UPageSection
      id="features"
      :ui="{
        root: 'bg-yellow-300 dark:bg-black',
        title: 'dark:text-yellow-300 text-black ' ,
        description: 'dark:text-yellow-300 text-black'
      }"
      title="V1sages"
      description="Mais qui peut donc bien être cette personne ?"
    >
    <div class="flex justify-center">
      <UCard class="w-48 mx-2 dark:bg-yellow-300" v-if="person">
          <template #header>
            <img :src="`images/${person?.picture.name}`"/>
          </template>
      </UCard>
      <UCard class="w-48 mx-2 dark:bg-yellow-300" v-else>
          <template #header>
              <UProgress color="neutral" />
          </template>
      </UCard>
      
    </div>

    <div class="flex justify-center">
      <UButton
        v-for="person in ramdomPersons" size="xl" class="m-2"
        @click="guessPerson(person)"
        color="neutral"
      >{{person?.name}}</UButton>
      </div>
    </UPageSection>
  </div>
    <UModal
    :ui="{title: 'text-center'}"
    title="Félicitations"
    :description="`Il s'agit bien de ${ guessedPerson?.name }, sans cette personne, nous manquerions cruellement de ${guessedPerson?.jobTitle}`"
    v-model:open="isModalOpen"
  >
  </UModal>
</template>
