<script lang="ts">
  import type IUsuario from "src/interfaces/IUsuario";
  import { createEventDispatcher } from "svelte";

  let valorInput = '';
  const dispatch = createEventDispatcher<{
    aoAlterarUsuario: IUsuario
  }>();
  
  async function aoSubmeter() {

    const respostaUsusario = await fetch(`https://api.github.com/users/${valorInput}`);
    const dadosUsuario = await respostaUsusario.json();

    dispatch('aoAlterarUsuario', {
      login: dadosUsuario.login,
      nome: dadosUsuario.name,
      avatar_url: dadosUsuario.avatar_url,
      perfil_url: dadosUsuario.html_url,
      repositorios_publicos: dadosUsuario.public_repos,
      seguidores: dadosUsuario.followers
    });

  }
</script>

<form on:submit|preventDefault={aoSubmeter}>
  <!-- on: DIRETIVA, submit: EVENTO -->
  <!-- pipes | modificadores de evento -->
  <input type="text" class="input" bind:value={valorInput} placeholder="Pesquise o nome do usuário">
  <div class="botao-container">
    <button type="submit" class="botao"> Buscar </button>
  </div>
</form>

<style>

.input {
    padding: 15px 25px;
    width: calc(100% - 8.75rem);
    font-size: 1rem;
    border-radius: 8px;
    border: 1px solid #2e80fa;
    box-shadow: 0px 17px 52px rgba(222, 231, 247, 0.4);
    outline: 0;
  }

  .input::placeholder {
    font-family: "Roboto";
    font-style: italic;
    font-weight: 300;
    font-size: 19.5px;
    line-height: 26px;
    color: #6e8cba;
  }

  .botao-container {
    position: absolute;
    width: 9.625rem;
    right: 0;
    top: 0;
    bottom: 0;
    display: flex;
  }

  .botao {
    padding: 15px 24px;
    border-radius: 8px;
    border: none;
    background: #2e80fa;
    line-height: 26px;
    color: #fff;
    font-size: 22px;
    cursor: pointer;

    transition: background-color 0.2s;

    display: flex;
    align-items: center;
    gap: 13px;
  }

  .botao:hover {
    background: #4590ff;
  }
</style>