# Simulador de Robô

Simulador interativo para aprender os fundamentos de robótica na sala de aula. Roda direto no navegador, sem instalação e sem dependências.

Feito para turmas do ensino técnico em Informática, mas serve para qualquer pessoa que queira entender como um robô lê o ambiente, decide e age.

## Modos

### Programar o robô

O aluno monta um programa com blocos e acompanha o robô executando no quadro, com o bloco atual destacado na lista. São 15 fases, com blocos liberados aos poucos.

**Blocos**

| Bloco | O que faz | Liberado a partir da fase |
|-------|-----------|---------------------------|
| Avançar, Girar à esquerda, Girar à direita | Movimentos básicos | 1 |
| Repetir (2 a 10 vezes) e Fim | Laço de repetição, que pode ser aninhado | 3 |
| Repetir até a bandeira | Laço que só termina quando o robô chega ao destino | 12 |
| Se houver obstáculo à frente e Senão | Decisão com base no sensor frontal | 12 |

**Fases**

| Fase | Nome | Desafio | Limite de blocos |
|------|------|---------|------------------|
| 1 | Primeiros passos | Chegar à bandeira com um exemplo pronto | sem limite |
| 2 | Desvio | Contornar um obstáculo | sem limite |
| 3 | Escada | Subir uma escada de obstáculos | 8 |
| 4 | Volta no bloco | Contornar um bloco grande em três trechos retos | 12 |
| 5 | Labirinto | Percorrer um corredor em zigue-zague | 22 |
| 6 | Slalom | Desviar de três obstáculos em fila | 10 |
| 7 | Perímetro | Seguir o contorno, com um Repetir dentro de outro | 6 |
| 8 | Baterias | Recolher 3 baterias e chegar à bandeira | sem limite |
| 9 | Degraus largos | Recolher baterias em uma escada de degraus largos | 7 |
| 10 | Varredura | Recolher 19 baterias varrendo a área linha por linha | 14 |
| 11 | Tesouro | Planejar uma rota longa em um labirinto com 3 baterias | sem limite |
| 12 | Siga a parede | Primeiro programa com sensor: gire se houver obstáculo, senão avance | 8 |
| 13 | Espiral | A mesma regra em um mapa novo | 8 |
| 14 | Pule os obstáculos | Contornar obstáculos em posições diferentes | 14 |
| 15 | Corrida de obstáculos | Desvio por baixo, recolhendo 3 baterias no caminho | 14 |

A partir da fase 3, o limite de blocos obriga o uso de laços. Nas fases 12 a 15, o limite só é atingido com um programa que reage ao sensor, em vez de decorar o caminho.

**Recursos da interface**

- Nota de 1 a 3 estrelas por fase, de acordo com a meta de blocos. A meta é a solução de referência, e é possível fazer melhor.
- Dica em cada fase, escondida até o aluno pedir.
- Desfazer, limpar e inserir blocos no meio do programa (toque em um bloco para escolher onde o próximo entra).
- Controle de velocidade da execução.
- O programa de cada fase é mantido ao trocar de fase.
- Progresso e última fase salvos no navegador. O botão "Apagar progresso" zera tudo, com confirmação.
- Proteção contra laços sem fim: o programa é parado após 400 ações.
- Painel com a leitura do sensor frontal, se há obstáculo à frente e quantas baterias foram recolhidas.

### Rota autônoma

Um robô aspirador percorre uma sala sorteada, com obstáculos, durante 200 passos. A regra é simples: se o sensor de distância indicar menos de 20 cm, o robô gira; caso contrário, avança. O aluno escolhe a estratégia de giro e compara a cobertura da sala em cada uma:

- sempre girar à direita
- sempre girar à esquerda
- sortear o lado

Há botões para executar, executar um passo por vez e gerar uma sala nova. A estratégia com maior cobertura fica destacada.

## Como usar

### Localmente

Baixe o repositório e abra o arquivo `index.html` no navegador.

```bash
git clone https://github.com/tainara-m/simulador-robo.git
cd simulador-robo
# abra index.html no navegador
```

### No GitHub Pages

1. Em **Settings > Pages**, escolha a branch `main` e a pasta `/ (root)`.
2. Salve. A página fica disponível em `https://tainara-m.github.io/simulador-robo/`.

## Sugestão de uso em aula

1. Peça que os alunos resolvam as fases 1 a 3 individualmente.
2. Nas fases 4 a 7, discuta em dupla qual padrão se repete antes de montar o programa.
3. Nas fases 8 a 11, peça que planejem a rota no papel antes de montar os blocos.
4. Nas fases 12 a 15, compare a regra com sensor com o programa decorado das fases anteriores: o que muda quando o mapa muda?
5. No modo autônomo, cada dupla testa as três estratégias na mesma sala e anota a cobertura de cada uma.
6. Feche com a discussão: por que sempre girar para o mesmo lado deixa áreas sem limpar?

A regra do modo autônomo, em C++ para Arduino com um sensor HC-SR04:

```cpp
void loop() {
  long d = lerDistanciaCm();
  if (d < 20) {
    girarDireita();
  } else {
    avancar();
  }
}
```

## Tecnologias

- HTML, CSS e JavaScript puros em um único arquivo
- Canvas 2D para o desenho do quadro e do robô
- Fontes Chakra Petch, IBM Plex Sans e IBM Plex Mono, carregadas do Google Fonts
- Tema claro e escuro automáticos, de acordo com o sistema
- Layout responsivo, com uso em celular e tablet

## Estrutura

```
.
├── index.html   # aplicação completa (HTML, CSS e JS)
├── LICENSE
└── README.md
```

Dentro do `index.html`, a lógica do jogo (interpretador dos blocos, regras de movimento e nota) fica entre os comentários `LOGIC-START` e `LOGIC-END`, separada do desenho e da interface.

## Personalização

As fases ficam na constante `LEVELS`, dentro do `<script>`. Cada fase define o tamanho do quadro, a posição e a direção inicial do robô, a bandeira, as paredes, as baterias, o limite de blocos, a meta, os blocos liberados e os textos de ajuda:

```js
{tit:'Slalom', w:7, h:7,
 start:{x:0, y:6, d:1},          // d: 0 cima, 1 direita, 2 baixo, 3 esquerda
 goal:{x:6, y:6},
 walls:[[1,6],[3,6],[5,6]],
 items:[[2,5]],                  // baterias (opcional)
 limit:10,                       // null para não limitar
 par:10,                         // meta de blocos, usada na nota
 blocks:B2,                      // B1 básicos, B2 com Repetir, B3 com sensor
 info:'Texto exibido ao aluno.', dica:'Texto da dica.'}
```

Os auxiliares `rect(x0,x1,y0,y1)`, `line(x0,y0,x1,y1)` e `except(w,h,cells)` ajudam a desenhar paredes. `except` marca como parede tudo o que não está na lista de células livres, o que facilita criar corredores.

Antes de publicar uma fase nova, confirme que existe uma solução dentro do limite de blocos e defina `par` com o tamanho dela.

## Licença

Distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE).
