<template>
    <Message :msg="msg" v-show="msg" />
    <div>
        <form id="burger-form" @submit="createBurger">
            <div class="input-container">
                <label>Seu nome:</label>
                <input type="text" id="nome" v-model="nome" placeholder="Digite o seu nome">
            </div>
            <div class="input-container">
                <label>Seu endereço:</label>
                <input type="text" id="endereco" v-model="endereco" placeholder="Digite o seu endereço">
            </div>
            <div class="input-container">
                <label>Escolha a forma de pagamento:</label>
                <select id="formaPagamento" v-model="formaPagamento">
                    <option value="" disabled selected>Selecione</option>
                    <option value="pix"> PIX </option>
                    <option value="cartaoCredito"> Cartão de crédito </option>
                    <option value="dinheiro"> Dinheiro </option>
                </select>
            </div>
            <div class="input-container">
                <label>Escolha o lanche:</label>
                <select id="lanche" v-model="lanche">
                    <option value="" disabled selected>Selecione</option>
                    <option v-for="lanche in lanches" :key="lanche.id" :value="lanche"> {{ lanche.nome }} (R$ {{ lanche.preco }}) </option>
                </select>
            </div>
            <div class="input-container">
                <label>Escolha a bebida:</label>
                <select id="bebida" v-model="bebida">
                    <option value="" disabled selected>Selecione</option>
                    <option v-for="bebida in bebidas" :key="bebida.id" :value="bebida">{{ bebida.nome }} (R$ {{ bebida.preco }})</option>
                </select>
            </div>
            <div id="acompanhamentos-container" class="input-container">
                <label id="acompanhamentos-title">Selecione os acompanhamentos:</label>
                <div class="checkbox-container" v-for="acompanhamento in acompanhamentos" :key="acompanhamento.id">
                    <input type="checkbox" :value="acompanhamento" v-model="acompanhamentosSelecionados">
                    <span>{{ acompanhamento.nome }} (R$ {{ acompanhamento.preco }})</span>
                </div>
            </div>
            <div>
                <input class="submit-btn" type="submit" value="Enviar para o carrinho!" :disabled="!nome || !endereco || !formaPagamento || !lanche || !bebida || !acompanhamentos">
            </div>
        </form>
    </div>
</template>

<script>
import Message from './Message.vue';

export default {
    name: "BurgerForm",
    data() {
        return {
            lanches: [],
            bebidas: [],
            acompanhamentos: [],
            nome: '',
            endereco: '',
            formaPagamento: '',
            lanche: '',
            bebida: '',
            acompanhamentosSelecionados: [],
            msg: null
        }
    },
    methods: {
        async getProdutos() {
            const req = await fetch("http://localhost:8080/api/produtos", {
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json'
                }
            })

            const data = await req.json();

            this.lanches = data.filter(produto => produto.categoria === "LANCHE")
            this.bebidas = data.filter(produto => produto.categoria === "BEBIDA")
            this.acompanhamentos = data.filter(produto => produto.categoria === "ACOMPANHAMENTO")
        },

        async createBurger(e) {

            e.preventDefault()

            const data = {
                nome: this.nome,
                endereco: this.endereco,
                formaPagamento: this.formaPagamento,
                lanche: this.lanche,
                bebida: this.bebida,
                acompanhamentos: this.acompanhamentosSelecionados
            }

            const dataJson = JSON.stringify(data)
            // console.log(dataJson)

            this.msg = `Pedido adicionado ao carrinho!`

            setTimeout(() => {
                this.msg = ""
                this.nome = "";
                this.lanche = "";
                this.bebida = "";
                this.acompanhamentosSelecionados = [];
            }, 4000);

        }
    },
    mounted() {
        this.getProdutos();
    },
    components: {
        Message
    }
}
</script>

<style scoped>
span {
    position: relative;
    margin-left: 2px;
}

input {
    position: relative;
    margin-top: 2px;
}

#burger-form {
    max-width: 400px;
    margin: 0 auto;
}

.input-container {
    display: flex;
    flex-direction: column;
    position: relative;
    left: 12%;
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

#acompanhamentos-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#acompanhamentos-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 40%;
    margin-bottom: 20px;
}

.checkbox-container spam,
.checkbox-container input {
    width: auto;
}

.checkbox-container spam {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    position: relative;
    left: 13%;
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>