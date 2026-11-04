# Simulador de Robô

Simulador interativo para aprender os fundamentos de robótica na sala de aula. Roda direto no navegador, sem instalação e sem dependências.

Feito para turmas do ensino técnico em Informática, mas serve para qualquer pessoa que queira entender como um robô lê o ambiente, decide e age.

## Modos

### Programar o robô

O aluno monta um programa com blocos e acompanha o robô executando no quadro. Os blocos disponíveis são avançar, girar à esquerda, girar à direita, repetir (de 2 a 6 vezes) e fim do repetir. O painel mostra a leitura do sensor frontal e a quantidade de blocos usados.

| Nível | Nome | Desafio | Limite de blocos |
|-------|------|---------|------------------|
| 1 | Primeiros passos | Chegar à bandeira com um exemplo pronto | sem limite |
| 2 | Desvio | Contornar um obstáculo | sem limite |
| 3 | Escada | Subir uma escada de obstáculos | 8 |
| 4 | Volta no bloco | Contornar um bloco grande em três trechos retos | 12 |
| 5 | Labirinto | Percorrer um corredor em zigue-zague | 22 |
| 6 | Slalom | Desviar de três obstáculos em fila | 10 |
| 7 | Perímetro | Seguir o contorno, com um Repetir dentro de outro | 6 |

A partir do nível 3, o limite de blocos obriga o uso de laços de repetição. O nível 7 exige laços aninhados.

### Rota autônoma

Um robô aspirador percorre uma sala sorteada, com obstáculos, durante 200 passos. A regra é simples: se o sensor de distância indicar menos de 20 cm, o robô gira; caso contrário, avança. O aluno escolhe a estratégia de giro e compara a cobertura da sala em cada uma:

- sempre girar à direita
- sempre girar à esquerda
- sortear o lado

O botão "Nova sala" gera outro cenário, e o controle de velocidade permite acompanhar com calma ou ver o resultado rápido.

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

1. Peça que os alunos resolvam os níveis 1 a 3 individualmente.
2. Nos níveis 4 a 7, discuta em dupla qual padrão se repete antes de montar o programa.
3. No modo autônomo, cada dupla testa as três estratégias na mesma sala e anota a cobertura de cada uma.
4. Feche com a discussão: por que sempre girar para o mesmo lado deixa áreas sem limpar?

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

## Personalização

Os níveis ficam na constante `LEVELS`, dentro do `<script>` do `index.html`. Cada nível define o tamanho do quadro, a posição e a direção inicial do robô, a bandeira, as paredes, o limite de blocos e o texto de ajuda:

```js
{nome:'Nível 6', tit:'Slalom', w:7, h:7,
 start:{x:0, y:6, d:1},   // d: 0 cima, 1 direita, 2 baixo, 3 esquerda
 goal:{x:6, y:6},
 walls:[[1,6],[3,6],[5,6]],
 limit:10,                // null para não limitar
 info:'Texto exibido ao aluno.', pre:[]}
```

Antes de publicar um nível novo, confirme que existe uma solução dentro do limite de blocos.

## Licença

Distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE).
