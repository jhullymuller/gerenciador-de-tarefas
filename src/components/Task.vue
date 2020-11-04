<template>
    <div id="task">
        <form @submit.prevent="addTarefa">
            <input   type= "text"
              placeholder="Tarefa de hoje?"
              v-model="tarefa"/>
            <button type="submit">Adicionar</button>
        </form>
        <Item :lista="tarefas" :delete="deleteTarefas" />

        <span v-show="tarefas.length > 0 ">
            Você tem <strong :class="{pend: pendente }">{{tarefas.length}}</strong> pendentes.
        </span>
   
   
    </div>
</template>

<script>
import Item from './Item';
export default {
    name:'Task',
    components:{
        Item
    },
    data(){
        return{
            tarefa: '',
            tarefas: [],
            pendentes: false
        }
    },
    methods:{
        addTarefa(){
            if(this.tarefa !== ''){
                this.tarefas.push({
                    text:this.tarefa,
                    key: Date.now(),
                });
            }else{
                alert('Digite uma tarefa...');
                return;
            }
            this.tarefa = '';
            console.log(this.tarefas);
        },
        deleteTarefas(key){
            let filtro = this.tarefas.filter( (item) => {
                return(item.key !== key);
            });
            return this.tarefas = filtro;
        }
    },
    watch:{
        tarefas:{
            deep: true,
            handler(){
            localStorage.setItem('tasks', JSON.stringify(this.tarefas));
            this.tarefas.length > 4 ? this.pendente = true : this.pendente = false;
           }
            
        }
    },
    created(){
        const minhalista = localStorage.getItem('tasks');
        this.tarefas = JSON.parse(minhalista)|| [];
    }

    
}
</script>
<style scoped>
#task{
    max-width: 700px;
    background: #FFFF;
    border-radius: 4px;
    padding: 20px;
    margin: 20px auto ;
    box-shadow: 0 0 20px rgb(0, 0, 0, 0.3);
}
 form{
        margin-top: 30px;
        display: flex;
        flex-direction:row;
    }
    form button{
        cursor: pointer;
        background: #0f5959;
        border: 0;
        border-radius: 4px;
        margin-left: 10px;
        padding: 8px 8px;
        display: flex;
        justify-content: center;
        justify-items: center;
        color:#FFFF;

    }
    input{
        flex: 1;
        border: 1px solid #eee;
        padding: 6px 10px;
        border-radius: 4px;
        font-size: 14px;
        outline: none;
    }
    .pend{
        color: red;
    }
</style>