<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form @submit="createBurger" id="burger-form">
                <div class="input-container">
                    <label for="name">Nome do Cliente:</label>
                    <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome" required>
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o pão:</label>
                    <select name="bread" id="bread" v-model="bread" required>
                        <option value="">Selecione o seu pão</option>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Escolha a carne do seu Burger:</label>
                    <select name="meat" id="meat" v-model="meat" required>
                        <option value="">Selecione a carne</option>
                        <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
                    </select>
                </div>
                <div class="input-container" id="opcionais-container">
                    <label id="optional-title" for="optional">Escolha os opcionais:</label>
                    <div class="checkbox-container" v-for="optional in optionalsData" :key="optional.id">
                        <input type="checkbox" name="optional" id="optional" :value="optional.tipo"
                            v-model="optionals"><span>{{ optional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from './shared/Message.vue';

export default {
    name: 'BurgerForm',
    components: {
        Message
    },
    data() {
        return {
            breads: null,
            meats: null,
            optionalsData: null,
            name: '',
            bread: '',
            meat: '',
            optionals: [],
            msg: null
        }
    },
    methods: {
        async getIgredientes() {
            const req = await fetch('http://localhost:3000/ingredientes');
            const data = await req.json();

            this.breads = data.paes;
            this.meats = data.carnes;
            this.optionalsData = Array.from(data.opcionais);
        },

        async createBurger(event) {
            event.preventDefault();

            const data = {
                nome: this.name,
                carne: this.meat,
                pao: this.bread,
                opcionais: this.optionals,
                status: 'Solicitado'
            }

            const dataJson = JSON.stringify(data);
            const req = await fetch('http://localhost:3000/burgers', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: dataJson
            });

            const res = await req.json();
            this.msg = `Pedido n° ${res.id} realizado com sucesso!`;
            this.name = '';
            this.bread = '';
            this.meat = '';
            this.optionals = '';

            setTimeout(() => {
                this.msg = null;
            }, 3000);
        }
    },
    mounted() {
        this.getIgredientes();
    }
}
</script>

<style scoped>
#burger-form {
    max-width: 400px;
    margin: 0 auto;
}

.input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;
}

input,
select {
    padding: 5px 10px;
    width: 300px;
}

#opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#optional-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transform: .5s;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>