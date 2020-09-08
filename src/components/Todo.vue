<template>
    <section class="todoapp container">
        <header class="header">
            <h1>To Do List</h1>
            <!--keyup => permet  la validation avec entrée-->
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo"
                   @keyup.enter="addTodo">
        </header>
        <!--vue-->
        <div class="main">
            <input type="checkbox" class="toggle-all" v-model="allDone">
            <!--bouton qui peret de cocher toutes les taches => classe associée a une propriété de la vue-->
            <ul class="todo-list">
                <!--boucle sur le li-->
                <!--v-bind completed => permet d'indiquer que la tache est completée (change l'apparence) et l'edition de celle ci-->
                <!--filteredTodos méthode dans le state pour récuparation des filtres-->
                <li class="todo" v-for="todo in filteredTodos"
                    v-bind:class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <!--completed => permet de connaitre l'etat de complétion d'un champ-->
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <!--declencher l edition d'une tache avec dblclick-->
                        <label @dblclick="editTodo(todo)"> {{todo.name}} </label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <!--edition d'un champ , associée a une nouvelle propriété editing -->
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit"  @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
                </li>
            </ul>
        </div>
        <!--v-show permet de faire disparaitre le footer lorsque toutes les tâches sont supprimées ( propriété hasTodo permet de faire réapparaire-->
        <footer class="footer" v-show="hasTodos">
            <!--la propriété remaining permet de compter le nbre de taches restantes-->
            <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
            <ul class="filters">
                <!--selected = element actif-->
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter= 'all'"> Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">> A faire</a>
                </li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">> Faites</a>
                </li>
            </ul>
            <!--le v-show fait apparaitre le button lorsque les tâches sont réalisées-->
            <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted"> Supprimer les tâches
                finies
            </button>
        </footer>
    </section>

</template>

<script>

    export default {
// <!--declaration du state avec les propriétés-->
        data() {
            return {
                todos: [{
                    name: 'tâche de test',
                    completed: false
                }],
                newTodo: '',
                // conservation dans le state du filtre au niveau du template
                filter: 'all',
                // propriété pour éditer une tache qui sauvergardera la tache modifiée
                editing: null,
                oldTodo: ''
            }
        },
        methods: {
            // <!--ajout d'une tache a la liste-->
            addTodo() {
                this.todos.push({
                    completed: false,
                    name: this.newTodo
                })

                // une fois la tache poussée ==> le champ se vide
                this.newTodo = ''
            },
            deleteTodo(todo) {
                this.todos = this.todos.filter(i => i != todo)
            },
            // suppresion des tâches réalisées
            deleteCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed)
            },
            editTodo(todo) {
                this.editing = todo
                this.oldTodo = todo.name
            },
            // methode permet de quitter apres edition d une tache
            doneEdit() {
                this.editing = null
            },
            cancelEdit(){
                this.editing = null
                this.editing.name= this.oldTodo
                this.doneEdit()
            }
        },
        // compteur de tâches, avec cette propriété ça peut servir d'avoir getter & setter
        computed: {
            // allDone est dans un v-model qui va avoir des getter et des setter ( évite de passer par une fonction ? )
            allDone: {
                // le get permet de définir comment est la proriété
                get() {
                    // pour eviter qu \'il reste coché en permanence ( car reflète l'état du remaining)
                    return this.remaining === 0
                },
                // le set permet de dire ce que je dois faire lorsque la propriété est modifiée
                set(value) {
                    if (value === true) {
                        this.todos.forEach(todo => {
                            todo.completed = value
                        })
                    }
                }
            },
            // méthode pour comptabiliser le nbre de taches
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            // prend la liste et fait un filtre dessus et retourne les complétées
            completed() {
                return this.todos.filter(todo => todo.completed).length
            },
            hasTodos() {
                return this.todos.length > 0
            },
            // recupération des filtres
            filteredTodos() {
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos
            }
        },
        // creation de directive avec des objets
        directives: {
            focus(el, value) {
                if (value) {
                    // nextTick : a la prochaine MAJ du dom adoptera ce comportement focus
                    Vue.nextTick( _ => {
                        el.focus()
                    })
                }
            }
        }
    }
</script>

<style src="./todo.css"></style>