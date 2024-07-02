<script>
    // Liste tous les statuts possibles pour notre todolist
    // DONE 1 : ajoute les 3 statuts dans la liste
    const available_status = ['à faire', 'fini', 'en cours'];
   
    // Initialise avec des valeurs par défaut
    let current_status = available_status[0];
    let all_tasks = [{
        id: 1,
        value: "tache de test 1",
        status: available_status[0]
    },{
        id: 2,
        value: "tache de test 2",
        status: available_status[0]
    },{
        id: 3,
        value: "tache de test 3",
        status: available_status[0]
    }];
    let preview = '';
  
  
  
  
    // Fonction appelée lors de la soumission du formulaire
    // DONE 4 : complète la fonction
    const addTask = (event) => {
        // DONE : évite le rechargement de la page
        event.preventDefault();
  
        // 1 - création d'une nouvelle tâche avec la valeur de preview
        const newTask = {
            id: all_tasks.length + 1,
            value: preview,
            status: available_status[0]
        };
  
        // 2 - on ajoute la tâche à la liste des tâches
        all_tasks.push(newTask);
  
        // Nécessaire pour que Svelte sache que le tableau a été modifié.
        // On réaffecte la variable avec un tableau recomposé à partir de son éclatement !
        all_tasks = all_tasks;
  
  
        // DONE : vide la valeur du champ input
        preview = '';
    }
  
  
    // Fonciton déclenchée lors du clic sur un bouton de filtrage
    const selectStatus = (value) => {
        current_status = value;
    }
  
  
    // Retourne un tableau contenant uniquement les tâches répondant à la condition "task.status est égal à status"
    const getFilteredTasks = (tasks, status) => {
        return tasks.filter((task) => task.status == status);
    }
  
  
    // DONE 6 : Retourne le nombre de tâches ayant le statut demandé
    const count = (tasks, status) => {
        // DONE : retourne la valeur en utilisant la fonction getFilteredTasks()
        // 1 - on récupère un tableau avec les tâches qui correspondent au statut
        const filteredTasks = getFilteredTasks(tasks, status);
        
        // 2 - on retourne le nombre de tâches dans le tableau
        return filteredTasks.length;
    }
  
  
    // DONE 7 : complète la fonction de changement de statut d'une tâche
    const migrateTask = (current_task, event) => {
        // DONE : change le statut de la tâche à partir du contexte de l'événment
        // current_task.status = ...;
        // 1 - on récupère le select à partir de l'objet event
        const selectElement = event.currentTarget;
        console.log(selectElement);
  
        // 2 - on récupère la valeur de l'élément
        const selectValue = selectElement.value;
        console.log(selectValue);
  
        console.log(current_task);
  
        // 3 - on met à jour la tâche
        current_task.status = selectValue;
        console.log(current_task);
       
        // Crée un tableau avec toutes les tâches sauf la courante
        let other_tasks = all_tasks.filter( task => task.id !== current_task.id);
       
        // Recrée un tableau contenant la tâche courante modifiée, plus les autres tâches
        all_tasks = [...other_tasks, current_task];
    }
  </script>
  
  
  
  
  <section class="todolist">
    <!-- DONE 3 : branche l'événement submit sur le formulaire pour appeler la fonction addTask() -->
    <form on:submit={addTask}>
        <label for="add-element">Description de la tâche</label>
  
  
        <!-- Bonus : branche la variable preview pour quelle reçoive la valeur de du champ input en temps réel -->
        <input bind:value={preview} required name="element" id="add-element" placeholder="Exemple : faire les courses"/>
        <!-- Bonus : affiche une prévisualisation de la tâche en cours d'ajout -->
        <span>Création en cours : {preview}</span>
  
  
        <button class="button" aria-label="Insérer une nouvelle tâche">Ajouter</button>
    </form>
  
  
  
  
    <nav aria-label="Filtrage des tâches">
        <!-- DONE 5 : ajoute les 3 boutons correspondant aux statuts (status prendra la valeur 'fini' par exemple)-->     
        <!-- BONUS : utilise une boucle #each pour parcourir available_status et créer les 3 boutons -->
        {#each available_status as status}
          <button aria-label="Statut '{status}'"
              on:click={(e)=> selectStatus(status)}
              class="button {status === current_status ? "active" : ""}">
              {status} ({count(all_tasks, status)})
          </button>
        {/each}
    </nav>
  
  
  
  
    <ul>
        <!-- Utilisation d'une boucle pour parcourir une liste -->
        <!-- La fonction getFilteredTasks() nous retourne une liste filtrée des tâches. La boucle la parcourt et ajoute un 'li' par élément -->
        {#each getFilteredTasks(all_tasks, current_status) as task}
            <li>
                <!-- DONE 2 : affiche le texte de la tâche -->
                <span>{task.value}</span>
  
  
                <!-- Affiche un sélecteur de statut pour le changer, en déclanchant la fonction migrateTask() -->
                <select on:change={(e) => migrateTask(task, e)} aria-label="Changer le statut de la tâche">
                    {#each available_status as status}
                        <option selected={status===current_status} value="{status}">{status}</option>
                    {/each}
                </select>
            </li>
        {/each}
    </ul>
  </section>