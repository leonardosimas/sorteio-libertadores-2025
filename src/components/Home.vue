<template>
  <v-app-bar :elevation="2" rounded>
    <!-- <template v-slot:prepend>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>
    </template> -->

    <v-app-bar-title class="appbar-title" style="font-family: Ubuntu">
      Simulador para o sorteio Libertadores 2025
    </v-app-bar-title>

    <RouterLink to="/About" style="text-decoration: none; color: inherit">
      <v-list-item :value="item" color="primary">
        <template v-slot:prepend>
          <v-icon icon="mdi-information"></v-icon>
        </template>
        <v-list-item-title :textContent="About"></v-list-item-title>
      </v-list-item>
    </RouterLink>

    <BtnLightDark />
  </v-app-bar>

  <v-container class="fill-height">
    <v-responsive class="align-center mx-auto" max-width="1200">
      <div class="text-center">
        <h2 class="text-h4 font-weight-bold">POTES</h2>
      </div>

      <div class="py-4" />

      <!-- Cards dos Potes -->
      <v-row justify="center">
        <v-col
          v-for="(times, pote) in potes || {}"
          :key="pote"
          cols="12"
          md="6"
          lg="3"
        >
          <CardPotes
            :pote="pote"
            :times="times"
            :sortearPote="() => sortearPote(pote)"
            :disabled="isButtonDisabled(pote)"
          />
        </v-col>
      </v-row>
    </v-responsive>

    <!-- Cards dos Grupos -->
    <v-row justify="center" class="mt-5">
      <CardGrupos
        v-for="(grupo, index) in grupos"
        :key="index"
        :grupo="'Grupo ' + String.fromCharCode(65 + index)"
        :times="grupo"
      />
    </v-row>
    <div v-if="todosSorteiosFinalizados || alertMessage">
      <v-btn @click="reiniciarSorteio" variant="outlined">
        Reiniciar Sorteio
      </v-btn>
    </div>
  </v-container>

  <!-- Alerta -->
  <Alert
    v-if="alertMessage"
    :text="alertMessage"
    :icon="alertIcon"
    :color="alertColor"
  />
</template>
  
  <script setup>
import { ref, onMounted } from "vue";
import "@/styles/Home.scss";
import "@fontsource/ubuntu";
import Alert from "./Alert.vue";
import BtnLightDark from "./btnLightDark.vue";

const potes = ref({});
const poteJaSorteado = ref([]); // Guardar quais potes já foram sorteados
const grupos = ref(Array(8).fill([]));
const timesRestantesPote1 = ref([]);
const timesRestantesPote2 = ref([]);
const timesRestantesPote3 = ref([]);
const timesRestantesPote4 = ref([]);
const grupoA = ref([]);
const grupoB = ref([]);
const grupoC = ref([]);
const grupoD = ref([]);
const grupoE = ref([]);
const grupoF = ref([]);
const grupoG = ref([]);
const grupoH = ref([]);
// Contadores de cliques por pote
const clicadoPote1 = ref(0);
const clicadoPote2 = ref(0);
const clicadoPote3 = ref(0);
const clicadoPote4 = ref(0);

// Estados dos botões
const botaoPote1Ativo = ref(true);
const botaoPote2Ativo = ref(false);
const botaoPote3Ativo = ref(false);
const botaoPote4Ativo = ref(false);

const todosSorteiosFinalizados = ref(false);

const alertMessage = ref(null); // Mensagem do alerta
const alertIcon = ref(null); // Ícone do alerta
const alertColor = ref(null); // Cor do alerta
const alertText = ref(null); // Cor do alerta

// Novas variáveis para controle de restrições
const brasil2Ultimos = ref(false);
const brasil3Ultimos = ref(false);
const argentina2Ultimos = ref(false);

const reiniciarSorteio = () => {
  // Reinicializa todos os estados do sorteio
  grupos.value = Array.from({ length: 8 }, () => []);
  poteJaSorteado.value = [];

  timesRestantesPote1.value = [...potes.value["Pote 1"]];
  timesRestantesPote2.value = [...potes.value["Pote 2"]];
  timesRestantesPote3.value = [...potes.value["Pote 3"]];
  timesRestantesPote4.value = [...potes.value["Pote 4"]];

  // Resetando os cliques
  clicadoPote1.value = 0;
  clicadoPote2.value = 0;
  clicadoPote3.value = 0;
  clicadoPote4.value = 0;

  todosSorteiosFinalizados.value = false;

  // Reseta os estados dos botões
  botaoPote1Ativo.value = true;
  botaoPote2Ativo.value = false;
  botaoPote3Ativo.value = false;
  botaoPote4Ativo.value = false;

  // Limpa os alertas
  alertMessage.value = null;
  alertIcon.value = null;
  alertColor.value = null;

  console.log("🔄 Sorteio reiniciado!");
};

// Função para exibir alertas
const mostrarAlerta = (text, icon, color) => {
  alertMessage.value = text;
  alertIcon.value = icon;
  alertColor.value = color;
  console.log(
    "alertMessage",
    alertMessage,
    "alertIcon",
    alertIcon,
    "alertColor",
    alertColor
  );
};

const carregarPotes = async () => {
  try {
    const response = await fetch("/potes.json");
    const data = await response.json();
    Object.assign(potes.value, data.Potes);
    timesRestantesPote1.value = [...potes.value["Pote 1"]];
    timesRestantesPote2.value = [...potes.value["Pote 2"]];
    timesRestantesPote3.value = [...potes.value["Pote 3"]];
    timesRestantesPote4.value = [...potes.value["Pote 4"]];
  } catch (error) {
    console.error("Erro ao carregar os potes:", error);
  }
};

// Inicializa os botões corretamente
botaoPote1Ativo.value = true;
botaoPote2Ativo.value = false;
botaoPote3Ativo.value = false;
botaoPote4Ativo.value = false;

console.log("botaoPote1Ativo", typeof botaoPote1Ativo);
console.log("botaoPote2Ativo", typeof botaoPote2Ativo);
console.log("botaoPote3Ativo", typeof botaoPote3Ativo);
console.log("botaoPote4Ativo", typeof botaoPote4Ativo);

// Função que é chamada ao clicar no botão de sorteio de cada Pote
const sortearPote = (pote) => {
  //   if (poteJaSorteado.value.includes(pote)) return;

  if (pote === "Pote 1") {
    sortearPosicaoPote1();
  } else if (pote === "Pote 2") {
    sortearPosicaoPote2();
  } else if (pote === "Pote 3") {
    sortearPosicaoPote3();
  } else if (pote === "Pote 4") {
    sortearPosicaoPote4();
  }

  // Marcar pote como sorteado
  poteJaSorteado.value.push(pote);
  console.log("pote", pote);
  console.log("poteJaSorteado", poteJaSorteado);

  // Verificar se todos os sorteios foram finalizados
  if (
    clicadoPote1 >= 7 &&
    clicadoPote2 >= 7 &&
    clicadoPote3 >= 7 &&
    clicadoPote4 >= 7
  ) {
    todosSorteiosFinalizados = ref(true);
    console.log("todosSorteiosFinalizados", todosSorteiosFinalizados);
  }
};

// Função para verificar se o botão do pote deve estar desabilitado
const isButtonDisabled = (pote) => {
  if (pote === "Pote 1") {
    return !botaoPote1Ativo.value;
  }
  if (pote === "Pote 2") {
    return !botaoPote2Ativo.value;
  }
  if (pote === "Pote 3") {
    return !botaoPote3Ativo.value;
  }
  if (pote === "Pote 4") {
    return !botaoPote4Ativo.value;
  }
  return true;
};

const sortearPosicaoPote1 = () => {
  if (clicadoPote1.value === 0) {
    const timeA1 = timesRestantesPote1.value.find(
      (time) => time.Posicao === "A1"
    );
    if (timeA1) {
      const timeComPosicao = { ...timeA1, Posicao: "A1" };
      grupos.value[0] = [timeComPosicao];
      grupoA.value = [timeComPosicao]; // Atualiza variável de controle

      timesRestantesPote1.value = timesRestantesPote1.value.filter(
        (time) => time.Posicao !== "A1"
      );
      clicadoPote1.value++;
      return;
    }
  }

  if (clicadoPote1.value > 0 && clicadoPote1.value < 7) {
    const randomIndex = Math.floor(
      Math.random() * timesRestantesPote1.value.length
    );
    const time = timesRestantesPote1.value[randomIndex];
    const grupoIndex = clicadoPote1.value;
    const grupoLetra = String.fromCharCode(65 + grupoIndex); // "B", "C", ..., "G"
    const timeComPosicao = { ...time, Posicao: `${grupoLetra}1` };

    grupos.value[grupoIndex] = [timeComPosicao];

    // Atualiza variável de controle correspondente ao grupo
    if (grupoLetra === "B") grupoB.value = [timeComPosicao];
    if (grupoLetra === "C") grupoC.value = [timeComPosicao];
    if (grupoLetra === "D") grupoD.value = [timeComPosicao];
    if (grupoLetra === "E") grupoE.value = [timeComPosicao];
    if (grupoLetra === "F") grupoF.value = [timeComPosicao];
    if (grupoLetra === "G") grupoG.value = [timeComPosicao];

    timesRestantesPote1.value.splice(randomIndex, 1);
    clicadoPote1.value++;
  }

  // O último time só é sorteado no oitavo clique
  if (clicadoPote1.value === 7) {
    clicadoPote1.value++; // Aguarda mais um clique antes de sortear o último time
    return;
  }

  if (clicadoPote1.value === 8) {
    const timeRestante = timesRestantesPote1.value[0];
    if (timeRestante) {
      const timeComPosicao = { ...timeRestante, Posicao: "H1" };
      grupos.value[7] = [timeComPosicao];
      grupoH.value = [timeComPosicao]; // Atualiza variável de controle

      timesRestantesPote1.value = [];

      //   verificarRestricoesPote1();

      // Desabilita o Pote 1
      botaoPote1Ativo.value = !botaoPote1Ativo.value;

      // Habilita o Pote 2
      botaoPote2Ativo.value = !botaoPote2Ativo.value;

      console.log("Sorteio do Pote 1 finalizado!");
    }
  }
};

// Função para verificar as restrições no final do Pote 1
// const verificarRestricoesPote1 = () => {
//   const timeF1 = grupoF.value[0]; // Posição F1
//   const timeG1 = grupoG.value[0]; // Posição G1
//   const timeH1 = grupoH.value[0]; // Posição H1

//   const brasileiros = [timeF1, timeG1, timeH1].filter(
//     (time) => time?.Pais === "Brasil"
//   ).length;
//   const argentinos = [timeG1, timeH1].filter(
//     (time) => time?.Pais === "Argentina"
//   ).length;

//   brasil2Ultimos.value =
//     brasileiros === 2 && timeG1?.Pais === "Brasil" && timeH1?.Pais === "Brasil";
//   brasil3Ultimos.value = brasileiros === 3;
//   argentina2Ultimos.value = argentinos === 2;

//   console.log("Restrição Brasil 2 Últimos:", brasil2Ultimos.value);
//   console.log("Restrição Brasil 3 Últimos:", brasil3Ultimos.value);
//   console.log("Restrição Argentina 2 Últimos:", argentina2Ultimos.value);
//   console.log("botaoPote2Ativo: ", botaoPote2Ativo.value);
// };

const sortearPosicaoPote2 = () => {
  if (clicadoPote2.value < 8) {
    // Sorteia um time aleatório
    const randomIndex = Math.floor(
      Math.random() * timesRestantesPote2.value.length
    );
    const time = timesRestantesPote2.value[randomIndex];

    console.log(`🎲 Sorteado: ${time.Nome} (${time.Pais})`);

    let grupoIndex = -1;
    let grupoLetra = "";

    // Encontra o primeiro grupo com apenas 1 time (garantindo ordem A → H)
    for (let i = 0; i < 8; i++) {
      if (grupos.value[i].length === 1) {
        grupoIndex = i;
        grupoLetra = String.fromCharCode(65 + i);
        break;
      }
    }

    if (grupoIndex === -1) {
      console.error("❌ Erro: Nenhum grupo disponível para alocar o time!");
      mostrarAlerta(
        "Erro: Nenhum grupo disponível para alocar o time!",
        "$error",
        "error"
      ); // Exibe alerta
      return;
    }

    // Contador para evitar loop infinito
    let tentativas = 0;
    const maxTentativas = 100;

    // Se houver conflito de país, realoca o time no próximo grupo disponível
    while (grupos.value[grupoIndex].some((t) => t.Pais === time.Pais)) {
      console.warn(
        `⚠️ Conflito detectado! ${time.Nome} (${time.Pais}) não pode ir para ${grupoLetra}2.`
      );

      grupoIndex++;
      if (grupoIndex > 7) grupoIndex = 7;
      grupoLetra = String.fromCharCode(65 + grupoIndex);

      console.warn(`🔄 Movendo ${time.Nome} para ${grupoLetra}2.`);

      tentativas++;
      if (tentativas >= maxTentativas) {
        console.error("❌ Erro crítico: Loop infinito detectado!");
        mostrarAlerta(
          "Erro crítico! Reiniciando sorteio...",
          "$error",
          "error"
        );
        // setTimeout(reiniciarSorteio, 2000);
        return;
      }
    }

    // Adiciona o time ao grupo final determinado
    const timeComPosicao = { ...time, Posicao: `${grupoLetra}2` };
    grupos.value[grupoIndex].push(timeComPosicao);
    eval(`grupo${grupoLetra}.value.push(timeComPosicao)`);

    console.log(`✅ ${time.Nome} foi alocado no Grupo ${grupoLetra}2.`);

    // Remove o time sorteado da lista
    timesRestantesPote2.value.splice(randomIndex, 1);
    clicadoPote2.value++;
  }

  // Após 8 cliques, finaliza o sorteio do Pote 2
  if (clicadoPote2.value === 8) {
    // Desabilita o Pote 2
    botaoPote2Ativo.value = !botaoPote2Ativo.value;

    // Habilita o Pote 3
    botaoPote3Ativo.value = !botaoPote3Ativo.value;
    console.log("🏆 Sorteio do Pote 2 finalizado!");

    // Exibe botão para reiniciar sorteio
    botaoReiniciarAtivo.value = true;

    // Log final dos grupos
    console.log("📊 Grupos após sorteio do Pote 2:");
    ["A", "B", "C", "D", "E", "F", "G", "H"].forEach((letra) => {
      console.log(`Grupo ${letra}:`, eval(`grupo${letra}.value`));
    });
  }
};

const sortearPosicaoPote3 = () => {
  if (clicadoPote3.value < 8) {
    // Sorteia um time aleatório
    const randomIndex = Math.floor(
      Math.random() * timesRestantesPote3.value.length
    );
    const time = timesRestantesPote3.value[randomIndex];

    console.log(`🎲 Sorteado: ${time.Nome} (${time.Pais})`);

    let grupoIndex = -1;
    let grupoLetra = "";

    // Encontra o primeiro grupo com apenas 2 times (garantindo ordem A → H)
    for (let i = 0; i < 8; i++) {
      if (grupos.value[i].length === 2) {
        grupoIndex = i;
        grupoLetra = String.fromCharCode(65 + i);
        break;
      }
    }

    if (grupoIndex === -1) {
      console.error("❌ Erro: Nenhum grupo disponível para alocar o time!");
      mostrarAlerta(
        "Erro: Nenhum grupo disponível para alocar o time!",
        "$error",
        "error"
      ); // Exibe alerta
      return;
    }

    let tentativas = 0; // Contador para evitar loop infinito
    const maxTentativas = 8; // Limite de tentativas para evitar loop

    // Se houver conflito de país ou grupo cheio, realoca o time no próximo grupo disponível
    while (
      (grupos.value[grupoIndex].some((t) => t.Pais === time.Pais) ||
        grupos.value[grupoIndex].length >= 3) &&
      tentativas < maxTentativas
    ) {
      console.warn(
        `⚠️ Conflito ou grupo cheio! ${time.Nome} (${time.Pais}) não pode ir para ${grupoLetra}3.`
      );

      grupoIndex++;
      if (grupoIndex > 7) grupoIndex = 0; // Volta para o grupo A se ultrapassar H
      grupoLetra = String.fromCharCode(65 + grupoIndex);

      console.warn(`🔄 Tentando alocar ${time.Nome} no grupo ${grupoLetra}3.`);
      tentativas++;
    }

    if (tentativas >= maxTentativas) {
      console.error("❌ Erro: Não foi possível alocar o time sem conflito!");
      mostrarAlerta(
        "Erro: Não foi possível alocar o time sem conflito!",
        "$error",
        "error"
      ); // Exibe alerta
      return;
    }

    // Adiciona o time ao grupo final determinado
    const timeComPosicao = { ...time, Posicao: `${grupoLetra}3` };
    grupos.value[grupoIndex].push(timeComPosicao);
    eval(`grupo${grupoLetra}.value.push(timeComPosicao)`);

    console.log(`✅ ${time.Nome} foi alocado no Grupo ${grupoLetra}3.`);

    // Remove o time sorteado da lista
    timesRestantesPote3.value.splice(randomIndex, 1);
    clicadoPote3.value++;
  }

  // Após 8 cliques, finaliza o sorteio do Pote 3
  if (clicadoPote3.value === 8) {
    // Desabilita o Pote 3
    botaoPote3Ativo.value = !botaoPote3Ativo.value;

    // Habilita o Pote 4
    botaoPote4Ativo.value = !botaoPote4Ativo.value;
    console.log("🏆 Sorteio do Pote 3 finalizado!");

    // Log final dos grupos
    console.log("📊 Grupos após sorteio do Pote 3:");
    ["A", "B", "C", "D", "E", "F", "G", "H"].forEach((letra) => {
      console.log(`Grupo ${letra}:`, eval(`grupo${letra}.value`));
    });
  }
};

const sortearPosicaoPote4 = () => {
  if (clicadoPote4.value < 8) {
    // Sorteia um time aleatório
    const randomIndex = Math.floor(
      Math.random() * timesRestantesPote4.value.length
    );
    const time = timesRestantesPote4.value[randomIndex];

    console.log(`🎲 Sorteado: ${time.Nome} (${time.Pais})`);

    let grupoIndex = -1;
    let grupoLetra = "";

    // Encontra o primeiro grupo com apenas 3 times (garantindo ordem A → H)
    for (let i = 0; i < 8; i++) {
      if (grupos.value[i].length === 3) {
        grupoIndex = i;
        grupoLetra = String.fromCharCode(65 + i);
        break;
      }
    }

    if (grupoIndex === -1) {
      console.error("❌ Erro: Nenhum grupo disponível para alocar o time!");
      mostrarAlerta(
        "Erro: Nenhum grupo disponível para alocar o time!",
        "$error",
        "error"
      ); // Exibe alerta
      botaoReiniciarAtivo.value = true; // Libera botão de reinício
      return;
    }

    let tentativas = 0; // Contador para evitar loop infinito
    const maxTentativas = 8; // Limite de tentativas para evitar loop

    if (time.pre === "true") {
      console.log(
        `✅ ${time.Nome} tem 'pre' ativado e foi alocado diretamente no Grupo ${grupoLetra}4.`
      );
    } else {
      let encontrouGrupo = false;

      while (!encontrouGrupo && tentativas < maxTentativas) {
        if (
          !grupos.value[grupoIndex].some((t) => t.Pais === time.Pais) &&
          grupos.value[grupoIndex].length < 4
        ) {
          encontrouGrupo = true;
        } else {
          console.warn(
            `⚠️ Conflito ou grupo cheio! ${time.Nome} (${time.Pais}) não pode ir para ${grupoLetra}4.`
          );

          grupoIndex++;
          if (grupoIndex > 7) grupoIndex = 0; // Volta para o grupo A se ultrapassar H
          grupoLetra = String.fromCharCode(65 + grupoIndex);

          console.warn(
            `🔄 Tentando alocar ${time.Nome} no grupo ${grupoLetra}4.`
          );

          tentativas++;
        }
      }

      if (!encontrouGrupo) {
        console.error("🚨 Erro crítico: Todos os grupos têm esse país!");
        mostrarAlerta(
          "Erro: O sorteio gerou conflitos nos grupos! Vamos reiniciar!",
          "$error",
          "error"
        ); // Exibe alerta
        botaoReiniciarAtivo.value = true; // Libera botão de reinício
        return;
      }
    }

    // Adiciona o time ao grupo final determinado
    const timeComPosicao = { ...time, Posicao: `${grupoLetra}4` };
    grupos.value[grupoIndex].push(timeComPosicao);
    eval(`grupo${grupoLetra}.value.push(timeComPosicao)`);

    console.log(`✅ ${time.Nome} foi alocado no Grupo ${grupoLetra}4.`);

    // Remove o time sorteado da lista
    timesRestantesPote4.value.splice(randomIndex, 1);
    clicadoPote4.value++;
  }

  // Após 8 cliques, finaliza o sorteio do Pote 4
  if (clicadoPote4.value === 8) {
    // Desabilita o Pote 4
    botaoPote4Ativo.value = !botaoPote4Ativo;
    mostrarAlerta(
      "Sucesso: Conseguimos sortear todos os times!",
      "$success",
      "success"
    ); // Exibe alerta
    console.log("🏆 Sorteio do Pote 4 finalizado!");

    // Log final dos grupos
    console.log("📊 Grupos após sorteio do Pote 4:");
    ["A", "B", "C", "D", "E", "F", "G", "H"].forEach((letra) => {
      console.log(`Grupo ${letra}:`, eval(`grupo${letra}.value`));
    });
  }
};

onMounted(carregarPotes);
</script>
  
  <style scoped>
.mt-5 {
  margin-top: 32px;
}

a {
  text-decoration: none;
  color: #ffffff;
}
</style>
  