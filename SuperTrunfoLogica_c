#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define TOTAL_ESTADOS 8                // Número de estados no jogo
#define CIDADES_POR_ESTADO 4           // Número de cidades por estado
#define TOTAL_CARTAS (TOTAL_ESTADOS * CIDADES_POR_ESTADO)  // Total de cartas geradas

typedef struct {
    char codigo[4];                // Código único da carta (ex: A01)
    char nome[50];                 // Nome da cidade
    unsigned long int populacao;   // População (tipo ampliado)
    float area;                    // Área em km²
    double pib;                    // PIB em bilhões
    int pontos_turisticos;         // Quantidade de pontos turísticos
    char estado[50];               // Nome do estado
    char pais[50];                 // Nome do país
    float densidade;               // Densidade populacional (população / área)
    float pib_per_capita;          // PIB per capita (PIB / população)
    float super_poder;             // Atributo agregado de 'Super Poder'
} Carta;

// Protótipos das funções usadas no programa
void gerarCartas(Carta cartas[]);
void compararCartas(Carta c1, Carta c2, int atributo);
void compararCartasAvancado(Carta c1, Carta c2, int atr1, int atr2);
void compararAtributosDetalhado(Carta c1, Carta c2);
void menuComparacao(Carta cartas[]);
void menuComparacaoAvancado(Carta cartas[]);
void removerNovaLinha(char *str);

int main() {
    // Inicializa a semente de números aleatórios com base no tempo atual
    srand((unsigned int)time(NULL));
    Carta cartas[TOTAL_CARTAS];

    // Gera as cartas com dados aleatórios e atributos derivados
    gerarCartas(cartas);

    // Exibe todas as cartas geradas, com índice para seleção posterior
    printf("\n=== Cartas Geradas ===\n");
    for (int i = 0; i < TOTAL_CARTAS; i++) {
        printf("\nÍndice %2d - Código: %s\n", i, cartas[i].codigo);
        printf("Cidade: %s, Estado: %s, País: %s\n",
               cartas[i].nome, cartas[i].estado, cartas[i].pais);
        printf("População: %lu\nÁrea: %.2f km²\nPIB: %.2lf bilhões\nPontos turísticos: %d\n",
               cartas[i].populacao,
               cartas[i].area,
               cartas[i].pib,
               cartas[i].pontos_turisticos);
        printf("Densidade: %.2f hab/km²\nPIB per Capita: %.2f\nSuper Poder: %.2f\n",
               cartas[i].densidade,
               cartas[i].pib_per_capita,
               cartas[i].super_poder);
    }

    // Menu principal para escolher o modo de comparação
    int escolha;
    printf("\nEscolha o modo de comparação:\n");
    printf("1 - Básico (um atributo)\n");
    printf("2 - Avançado (dois atributos)\n");
    printf("3 - Batalha (todos atributos + Super Poder)\n");
    printf("Outro - Sair\n");
    printf("Sua escolha: ");
    scanf("%d", &escolha);

    switch (escolha) {
        case 1:
            menuComparacao(cartas);          // Modo básico
            break;
        case 2:
            menuComparacaoAvancado(cartas);  // Modo avançado
            break;
        case 3: {
            int c1, c2;
            // Solicita duas cartas para a batalha completa
            printf("Digite os índices de duas cartas para a batalha (0 a %d): ", TOTAL_CARTAS - 1);
            scanf("%d %d", &c1, &c2);
            if (c1 >= 0 && c1 < TOTAL_CARTAS && c2 >= 0 && c2 < TOTAL_CARTAS) {
                compararAtributosDetalhado(cartas[c1], cartas[c2]);
            } else {
                printf("Índices inválidos!\n");
            }
            break;
        }
        default:
            printf("Saindo...\n");  // Opção de sair
            break;
    }

    return 0;  // Fim do programa
}

// Gera cartas aleatórias e calcula densidade, PIB per capita e Super Poder
void gerarCartas(Carta cartas[]) {
    const char estados[] = "ABCDEFGH";              // Letras para os estados
    const char *nomesCidades[] = {"Cidade1","Cidade2","Cidade3","Cidade4"};
    const char *nomePais = "PaísX";                  // Nome fixo para o país
    int index = 0;

    for (int e = 0; e < TOTAL_ESTADOS; e++) {
        char nomeEstado[50];
        sprintf(nomeEstado, "Estado %c", estados[e]);  // Formata nome do estado
        for (int c = 0; c < CIDADES_POR_ESTADO; c++) {
            // Monta código e nomes
            sprintf(cartas[index].codigo, "%c%02d", estados[e], c + 1);
            strcpy(cartas[index].nome, nomesCidades[c]);
            strcpy(cartas[index].estado, nomeEstado);
            strcpy(cartas[index].pais, nomePais);

            // Valores aleatórios para atributos
            cartas[index].populacao = (unsigned long int)(rand() % 9000000 + 1000000);
            cartas[index].area = (rand() % 9000 + 1000) / 10.0f;
            cartas[index].pib = (rand() % 100 + 10) / 10.0;
            cartas[index].pontos_turisticos = rand() % 10 + 1;

            // Cálculo de atributos derivados
            cartas[index].densidade = (float)cartas[index].populacao / cartas[index].area;
            cartas[index].pib_per_capita = (float)(cartas[index].pib * 1e9 / cartas[index].populacao);
            // Super Poder: soma de todos atributos numéricos + inverso da densidade
            cartas[index].super_poder = 
                (float)cartas[index].populacao + 
                cartas[index].area + 
                (float)cartas[index].pib + 
                (float)cartas[index].pontos_turisticos + 
                cartas[index].pib_per_capita + 
                (1.0f / cartas[index].densidade);

            index++;
        }
    }
}

// Função de comparação básica de um único atributo entre duas cartas
void compararCartas(Carta c1, Carta c2, int atributo) {
    float v1, v2;
    char nomeAttr[50];
    int inverter = 0;  // Flag para inverter lógica em caso de densidade (menor vence)

    // Define qual atributo comparar e se inverte a regra
    switch (atributo) {
        case 1: v1 = c1.populacao;        v2 = c2.populacao;        strcpy(nomeAttr, "População");         break;
        case 2: v1 = c1.area;             v2 = c2.area;             strcpy(nomeAttr, "Área");              break;
        case 3: v1 = c1.densidade;        v2 = c2.densidade;        strcpy(nomeAttr, "Densidade Pop."); inverter = 1; break;
        case 4: v1 = c1.pib;              v2 = c2.pib;              strcpy(nomeAttr, "PIB");               break;
        case 5: v1 = c1.pib_per_capita;   v2 = c2.pib_per_capita;   strcpy(nomeAttr, "PIB per Capita");    break;
        case 6: v1 = c1.pontos_turisticos;v2 = c2.pontos_turisticos;strcpy(nomeAttr, "Pontos Turísticos"); break;
        default:
            printf("Atributo inválido!\n");
            return;
    }

    // Exibe valores e determina vencedor
    printf("\nComparação: %s\n", nomeAttr);
    printf("%s: %.2f VS %s: %.2f\n", c1.nome, v1, c2.nome, v2);
    if (v1 == v2) printf("Empate!\n");
    else if ((v1 > v2 && !inverter) || (v1 < v2 && inverter))
        printf("Vencedor: %s\n", c1.nome);
    else
        printf("Vencedor: %s\n", c2.nome);
}

// Menu interativo para comparação básica
void menuComparacao(Carta cartas[]) {
    while (1) {
        int i1, i2, attr;
        printf("\n=== Comparação Básica ===\n");
        printf("Digite dois índices de cartas (0 a %d), ou -1 para sair: ", TOTAL_CARTAS - 1);
        scanf("%d %d", &i1, &i2);
        if (i1 == -1 || i2 == -1) break;  // sai do loop
        if (i1 < 0 || i1 >= TOTAL_CARTAS || i2 < 0 || i2 >= TOTAL_CARTAS) {
            printf("Índices inválidos!\n"); continue;
        }
        printf("Escolha o atributo (1-População, 2-Área, 3-Densidade, 4-PIB, 5-PIB per Capita, 6-Pontos Tur.): ");
        scanf("%d", &attr);
        compararCartas(cartas[i1], cartas[i2], attr);
    }
}

// Comparação avançada com dois atributos dinâmicos, soma e tratamento de empate
void compararCartasAvancado(Carta c1, Carta c2, int atr1, int atr2) {
    float v1a1, v2a1, v1a2, v2a2;
    int inv1 = 0, inv2 = 0;
    char n1[50], n2[50];

    // Macro para obter valor e nome do atributo
    #define OBTER_VALOR(c, atr, inv, nome) ({ \
        float _val; \
        switch (atr) { \
            case 1: _val = (float)c.populacao; strcpy(nome, "População"); inv = 0; break; \
            case 2: _val = c.area; strcpy(nome, "Área"); inv = 0; break; \
            case 3: _val = c.densidade; strcpy(nome, "Densidade Pop."); inv = 1; break; \
            case 4: _val = (float)c.pib; strcpy(nome, "PIB"); inv = 0; break; \
            case 5: _val = c.pib_per_capita; strcpy(nome, "PIB per Capita"); inv = 0; break; \
            case 6: _val = (float)c.pontos_turisticos; strcpy(nome, "Pontos Turísticos"); inv = 0; break; \
            default: _val = 0; strcpy(nome, ""); inv = 0; break; \
        } _val; })

    // Obtém valores para cada carta e atributo
    v1a1 = OBTER_VALOR(c1, atr1, inv1, n1);
    v2a1 = OBTER_VALOR(c2, atr1, inv1, n1);
    v1a2 = OBTER_VALOR(c1, atr2, inv2, n2);
    v2a2 = OBTER_VALOR(c2, atr2, inv2, n2);
    #undef OBTER_VALOR

    // Soma dos dois atributos
    float soma1 = v1a1 + v1a2;
    float soma2 = v2a1 + v2a2;

    // Exibe resultado detalhado
    printf("\n=== Comparação Avançada ===\n");
    printf("Carta 1 (%s): %s=%.2f, %s=%.2f, Soma=%.2f\n", c1.nome, n1, v1a1, n2, v1a2, soma1);
    printf("Carta 2 (%s): %s=%.2f, %s=%.2f, Soma=%.2f\n", c2.nome, n1, v2a1, n2, v2a2, soma2);
    if (soma1 == soma2) printf("Resultado: Empate!\n");
    else printf("Vencedor: %s\n", (soma1 > soma2) ? c1.nome : c2.nome);
}

// Menu interativo para comparação avançada
void menuComparacaoAvancado(Carta cartas[]) {
    while (1) {
        int i1, i2, a1, a2;
        printf("\n=== Comparação Avançada ===\n");
        printf("Índices (0 a %d) ou -1 para sair: ", TOTAL_CARTAS - 1);
        scanf("%d %d", &i1, &i2);
        if (i1 == -1 || i2 == -1) break;
        if (i1 < 0 || i1 >= TOTAL_CARTAS || i2 < 0 || i2 >= TOTAL_CARTAS) {
            printf("Índices inválidos!\n"); continue;
        }
        printf("Escolha o 1º atributo (1 a 6): "); scanf("%d", &a1);
        if (a1 < 1 || a1 > 6) { printf("Atributo inválido!\n"); continue; }
        printf("Escolha o 2º atributo (1 a 6, diferente do 1º): "); scanf("%d", &a2);
        if (a2 < 1 || a2 > 6 || a2 == a1) { printf("Atributo inválido!\n"); continue; }
        compararCartasAvancado(cartas[i1], cartas[i2], a1, a2);
    }
}

// Batalha completa, comparando cada atributo e exibindo 1 ou 0 conforme vencedor
void compararAtributosDetalhado(Carta c1, Carta c2) {
    printf("\n=== Batalha de Cartas ===\n");
    printf("População: Carta 1 venceu (%d)\n", c1.populacao > c2.populacao);
    printf("Área: Carta 1 venceu (%d)\n", c1.area > c2.area);
    printf("PIB: Carta 1 venceu (%d)\n", c1.pib > c2.pib);
    printf("Pontos Turísticos: Carta 1 venceu (%d)\n", c1.pontos_turisticos > c2.pontos_turisticos);
    printf("Densidade Populacional: Carta 1 venceu (%d)\n", c1.densidade < c2.densidade);
    printf("PIB per Capita: Carta 1 venceu (%d)\n", c1.pib_per_capita > c2.pib_per_capita);
    printf("Super Poder: Carta 1 venceu (%d)\n", c1.super_poder > c2.super_poder);
}

// Remove '\n' do final de uma string, se existir
void removerNovaLinha(char *str) {
    size_t len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}
