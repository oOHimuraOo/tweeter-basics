<script lang="ts">
import { logar, usuarioLogado } from '@/utils/mocks'
import { defineComponent, reactive } from 'vue'
import { RouterLink, useRouter } from 'vue-router'

const estado = reactive({
  user: '',
  senha: '',
  login: true,
})

//precisa adicionar o fetch posteriormente. Um fetch que verifica se existe ou não essa informação no banco de dados.
function verificar_login(u: string, s: string): boolean {
  logar(u, s)
  const logado: boolean = usuarioLogado(u)
  estado.login = logado
  if (logado) {
    localStorage.setItem('usuario', u)
    estado.user = ''
    estado.senha = ''
    const usuario = document.getElementById('user') as HTMLInputElement
    usuario.value = ''
    const senha = document.getElementById('password') as HTMLInputElement
    senha.value = ''
  } else {
    if (localStorage.getItem('usuario') !== null) {
      localStorage.removeItem('usuario')
    }
  }
  return logado
}

export default defineComponent({
  name: 'LoginComponent',
  setup() {
    const router = useRouter()

    function handleSubmit() {
      if (verificar_login(estado.user, estado.senha)) {
        router.push('/feed')
      }
    }

    return {
      estado,
      handleSubmit,
    }
  },
  components: {
    RouterLink,
  },
})
</script>

<template>
  <div class="container">
    <form action="">
      <h3>Logar</h3>
      <p v-if="!estado.login" class="visible">usuario ou senha incorretos!</p>
      <label for="user">user</label>
      <input type="text" id="user" v-model="estado.user" />
      <label for="password">password</label>
      <input type="password" id="password" v-model="estado.senha" />
      <button type="submit" @click.prevent="handleSubmit">Logar</button>
    </form>
    <router-link to="/register">registrar nova conta</router-link>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/Variaveis.scss';
@use 'sass:color';

form {
  display: flex;
  flex-direction: column;

  h3 {
    font-size: Variaveis.$fontSizeG;
    font-weight: bold;
    color: Variaveis.$corSecundaria;
    margin-bottom: 16px;
  }

  p {
    max-width: 220px;
    font-size: Variaveis.$fontSizePPP;
    margin-bottom: 8px;
    color: red;
    display: none;
  }

  .visible {
    display: block;
  }

  label {
    font-size: Variaveis.$fontSizeM;
    font-weight: bold;
    color: Variaveis.$corSecundaria;
    margin-bottom: 6px;
  }

  input {
    background-color: Variaveis.$corDeFundoPrimaria;
    border: 2px solid Variaveis.$corTerciaria;
    border-radius: 4px;
    margin-bottom: 6px;
    padding: 4px;
    color: Variaveis.$corSecundaria;
    outline: none;
  }

  button {
    background-color: Variaveis.$corDeFundoTerciaria;
    outline: none;
    border: none;
    border-radius: 8px;
    padding: 4px;
    margin: 12px;
    cursor: pointer;

    &:hover {
      background-color: color.adjust(
        Variaveis.$corDeFundoTerciaria,
        $lightness: -5%
      );
    }
  }
}

a {
  display: flex;
  justify-content: center;
  color: Variaveis.$corDeFundoTerciaria;
  font-size: Variaveis.$fontSizePPP;

  &:hover {
    color: color.adjust(Variaveis.$corDeFundoTerciaria, $lightness: -5%);
  }
}
</style>
