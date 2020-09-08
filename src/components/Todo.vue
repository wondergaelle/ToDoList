<template>
    <section class="todoapp container">
        <header class="header">
            <h1>To Do List</h1>
            <!--modification de la valeur new todo-->
            <!--keyup => permet  la validation avec entrée-->
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo"
                   @keyup.enter="addTodo">
        </header>

        <div class="main">
            <ul class="todo-list">
                <!--boucle sur le li-->
                <!--v-bind completed => permet d'indiquer que la tache est completée (change l'apparence)-->
                <!--filteredTodos methode dans le state pour récuparation des filtres-->
                <li class="todo" v-for="todo in filteredTodos" v-bind:class="{completed: todo.completed}">
                    <div class="view">
                        <!--completed => permet de connaitre l'etat de complétion d'un champ-->
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label> {{todo.name}} </label>
                    </div>
                </li>
            </ul>
        </div>
        <footer class="footer">
            <!--la propriété remaining permet de compter le nbre de taches restantes-->
            <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
            <ul class="filters">
                <!--selected = element actif-->
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter= 'all'"> Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}"@click.prevent="filter = 'todo'">> A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">> Faites</a></li>
            </ul>
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
                filter:'all'
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
            }
        },
        // compteur de tache
        computed: {
            // prend la liste et fait un filtre dessus et retourne les cons complétées
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            // recupération des filtres
            filteredTodos(){
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos
            }
        }
    }
</script>

<style src="./todo.css"></style>