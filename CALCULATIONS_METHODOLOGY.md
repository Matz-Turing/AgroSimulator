# Metodologia de Cálculos - Simulador Agrícola Digital

## 1. SIMULADOR DE IRRIGAÇÃO

### Fórmulas Utilizadas:

**Evapotranspiração de Referência (ET0) - Método Penman-Monteith Simplificado:**
```
ET0 = ((T + 17.8) × √max(T - UR, 1)) × 0.0023
```
- T = Temperatura média (°C)
- UR = Umidade relativa (%)
- Constante 0.0023 = fator de conversão para mm/dia

**Evapotranspiração da Cultura (ETc):**
```
ETc = ET0 × Kc
```
- Kc = Coeficiente da cultura por estágio:
  - Milho: Inicial(0.3), Desenvolvimento(0.7), Meio(1.2), Final(0.6)
  - Soja: Inicial(0.4), Desenvolvimento(0.8), Meio(1.15), Final(0.5)
  - Trigo: Inicial(0.4), Desenvolvimento(0.7), Meio(1.15), Final(0.4)

**Necessidade Líquida de Irrigação:**
```
NLI = (ETc × Fator_Solo) - Chuva_Efetiva
```
- Fator_Solo: Arenoso(0.8), Médio(1.0), Argiloso(1.2)
- Chuva_Efetiva = Precipitação × 0.8 (80% de eficiência)

**Volume de Irrigação:**
```
Volume_Diário = NLI × Área × 10 (litros/dia)
```

## 2. SIMULADOR DE PRODUTIVIDADE

### Metodologia:

**Produtividade Ajustada:**
```
Produtividade = Rendimento_Base × Fator_Ajuste_Total
```

**Fatores de Ajuste:**
- **Sementes:** Excelente(1.2), Boa(1.0), Regular(0.8), Ruim(0.6)
- **Solo:** Alta fertilidade(1.25), Média(1.0), Baixa(0.7)
- **Irrigação:** Gotejamento(1.3), Aspersão(1.2), Pivô(1.25), Sequeiro(0.8)
- **Clima:** Ótimo(1.15), Bom(1.0), Regular(0.85), Ruim(0.6)
- **Manejo:** Alto(1.2), Médio(1.0), Baixo(0.8)
- **Fitossanitário:** Excelente(1.1), Bom(1.0), Regular(0.9), Ruim(0.7)

**Rendimentos Base (kg/ha):**
- Milho: 8.000, Soja: 3.200, Trigo: 2.800, Arroz: 7.500, Feijão: 2.000

**Faixa de Variação:**
```
Mínimo = Produtividade × 0.85
Máximo = Produtividade × 1.15
```

## 3. SIMULADOR DE CUSTOS

### Cálculos:

**Custo Total por Categoria:**
```
Total_Categoria = Σ(Preço_Unitário × Quantidade)
```

**Custo por Hectare:**
```
Custo/ha = Custo_Total ÷ Área_Total
```

**Análise financeira simples com agrupamento por categorias de insumos**

## 4. CONTROLE DE PRAGAS

### Metodologia Científica:

**Dano Potencial:**
```
Dano = Dano_Base × Fator_Infestação × Fator_Estágio × Fator_Climático
```

**Fatores de Infestação:**
- Baixo: 0.3, Médio: 0.7, Alto: 1.0, Severo: 1.5

**Fatores de Estágio:**
- Vegetativo: 0.8, Floração: 1.2, Frutificação: 1.3, Maturação: 0.6

**Fatores Climáticos:**
- Seco: 0.8, Úmido: 1.2, Chuvoso: 1.1, Muito Quente: 1.3

**Eficácia do Tratamento:**
- Base: 85%, Com resistência: 60%, Eficaz anterior: 90%

**Custos por Hectare (R$):**
- Inseticida: 120, Biológico: 180, Sistêmico: 200, Fungicida: 150

## 5. RISCO CLIMÁTICO

### Análise Probabilística:

**Riscos Regionais Base:**
- Centro-Oeste: Seca(25%), Chuva excessiva(20%), Granizo(15%)
- Sul: Seca(20%), Chuva excessiva(30%), Granizo(35%), Geada(40%)
- Nordeste: Seca(60%), Estresse térmico(45%)

**Sensibilidade das Culturas:**
- Soja à seca: 0.9, Milho ao granizo: 0.9, Trigo à chuva excessiva: 0.8

**Risco Ajustado:**
```
Risco_Final = Risco_Regional × Sensibilidade_Cultura × Fator_Mitigação
```

**Fatores de Mitigação:**
- Gotejamento reduz risco de seca em 40%
- Solo argiloso reduz risco hídrico em 10%

**Risco Total Combinado:**
```
RT = 1 - Π(1 - Risco_Individual_i)
```

## 6. CRESCIMENTO DA CULTURA

### Modelo de Crescimento Sigmoidal:

**Função de Crescimento:**
```
Crescimento = 1 / (1 + e^(-k × (t - t0))) × Fatores_Ambientais
```
- k = taxa de crescimento (10 para modelagem rápida)
- t = tempo normalizado (0-1)
- t0 = ponto de inflexão (0.5)

**Parâmetros por Cultura:**
- **Soja:** Ciclo 120 dias, Altura máx. 80cm, Biomassa máx. 150g
- **Milho:** Ciclo 140 dias, Altura máx. 250cm, Biomassa máx. 400g
- **Tomate:** Ciclo 110 dias, Altura máx. 120cm, Biomassa máx. 200g

**Fatores Ambientais:**
```
Fator_Total = Fator_Temperatura × Fator_Umidade × Fator_Luz × Fator_Solo
```

**Coeficientes dos Fatores:**
- **Temperatura:** Ótima(1.0), Adequada(0.9), Limitante(0.7), Crítica(0.4)
- **Umidade:** Ideal(1.0), Boa(0.9), Baixa(0.6), Seca(0.3)
- **Luminosidade:** Alta(1.0), Média(0.8), Baixa(0.6)
- **Solo:** Fértil(1.0), Médio(0.8), Pobre(0.6)

**Estágios Fenológicos:**
- Germinação (0-10%), Vegetativo (10-50%), Floração (50-80%), Frutificação (80-100%)

## 7. FERTILIZAÇÃO

### Cálculo de Necessidade Nutricional:

**Fórmula Base:**
```
Necessidade = (Extração_Cultura / Eficiência_Fertilizante) × Fator_Solo × Produtividade_Esperada
```

**Extração por Tonelada Produzida (kg/t):**
- **Soja:** N(80), P₂O₅(15), K₂O(20)
- **Milho:** N(15), P₂O₅(6), K₂O(4)
- **Tomate:** N(3), P₂O₅(1), K₂O(4)

**Eficiência dos Fertilizantes:**
- Nitrogênio (N): 60%
- Fósforo (P₂O₅): 30%
- Potássio (K₂O): 80%

**Fatores de Correção do Solo:**
```
Fator_P = 1.0 + (max(0, 15 - P_solo) / 15) × 0.5
Fator_K = 1.0 + (max(0, 80 - K_solo) / 80) × 0.4
```

**Análise de Solo - Interpretação:**
- **Fósforo:** Baixo(<10), Médio(10-20), Alto(>20) mg/dm³
- **Potássio:** Baixo(<50), Médio(50-120), Alto(>120) mg/dm³
- **pH:** Ácido(<5.5), Adequado(5.5-6.5), Alcalino(>6.5)

**Necessidade de Calagem:**
```
NC = (V₂ - V₁) × CTC × f / 100
```
- V₁ = saturação atual, V₂ = saturação desejada (70%)
- CTC = capacidade de troca catiônica
- f = fator de correção (1.0 para calcário)

**Parcelamento de Aplicação:**
- **Nitrogênio:** 30% plantio + 70% cobertura
- **Fósforo:** 100% plantio
- **Potássio:** 50% plantio + 50% cobertura

## BASES CIENTÍFICAS:

1. **Irrigação:** Baseado em Allen et al. (1998) - FAO Paper 56
2. **Produtividade:** EMBRAPA e dados históricos regionais
3. **Pragas:** Literatura fitossanitária e limiares de dano econômico
4. **Clima:** Dados históricos INMET e modelos probabilísticos

## LIMITAÇÕES:

- Estimativas baseadas em médias históricas
- Não considera variações microclimáticas
- Requer validação com dados locais específicos
- Consulta agronômica recomendada para decisões críticas

---
*Desenvolvido por @Matz-Turing*
*Simulador Agrícola Digital v1.0*