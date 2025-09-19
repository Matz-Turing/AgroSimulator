# Simulador Agrícola Digital
## Documentação Técnica e Metodologia de Cálculos

###  **Visão Geral**

O Simulador Agrícola Digital é uma aplicação web moderna desenvolvida como ferramenta de apoio à decisão agronômica, integrando modelos matemáticos consolidados com uma interface intuitiva e interativa. O sistema combina rigor científico nos cálculos com facilidade de uso, permitindo simulações precisas de cenários agrícolas diversos.

---

##  **Arquitetura Técnica**

### **Stack Tecnológica**
- **Frontend:** React + TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **Visualização:** Recharts + Chart.js
- **Hospedagem:** Vercel (HTTPS nativo)
- **Armazenamento:** localStorage + sessionStorage

### **Características da Aplicação**
- **Arquitetura:** Single Page Application (SPA)
- **Responsividade:** Design adaptável para diferentes dispositivos  
- **Interface:** Dashboard interativo com formulários intuitivos
- **Persistência:** Armazenamento local no navegador (sem BD externo)
- **Segurança:** HTTPS nativo via Vercel

### **Módulos Integrados**
1. **Irrigação** - Cálculo de necessidades hídricas
2. **Crescimento de Culturas** - Modelagem fenológica
3. **Fertilização** - Recomendação de adubação NPK
4. **Produtividade** - Estimativa de rendimento
5. **Custos** - Análise econômica de produção
6. **Risco Climático** - Avaliação probabilística
7. **Controle de Pragas** - Manejo fitossanitário

---

## 📊 **Metodologia de Cálculos**

### **1. SIMULADOR DE IRRIGAÇÃO**

#### **Evapotranspiração de Referência (ET₀)**
*Método Penman-Monteith Simplificado*

```mathematica
ET₀ = ((T + 17.8) × √max(T - UR, 1)) × 0.0023
```

**Parâmetros:**
- `T` = Temperatura média (°C)
- `UR` = Umidade relativa (%)
- `0.0023` = Fator de conversão para mm/dia

#### **Evapotranspiração da Cultura (ETc)**

```mathematica
ETc = ET₀ × Kc
```

**Coeficientes da Cultura (Kc) por Estágio:**

| Cultura | Inicial | Desenvolvimento | Meio | Final |
|---------|---------|-----------------|------|-------|
| Milho   | 0.3     | 0.7            | 1.2  | 0.6   |
| Soja    | 0.4     | 0.8            | 1.15 | 0.5   |
| Trigo   | 0.4     | 0.7            | 1.15 | 0.4   |

#### **Necessidade Líquida de Irrigação**

```mathematica
NLI = (ETc × Fator_Solo) - Chuva_Efetiva
```

**Fatores de Correção:**
- **Solo Arenoso:** 0.8
- **Solo Médio:** 1.0  
- **Solo Argiloso:** 1.2
- **Chuva Efetiva:** Precipitação × 0.8 (80% de eficiência)

#### **Volume de Irrigação Diário**

```mathematica
Volume_Diário = NLI × Área × 10 (litros/dia)
```

---

### **2. SIMULADOR DE PRODUTIVIDADE**

#### **Produtividade Ajustada**

```mathematica
Produtividade = Rendimento_Base × Fator_Ajuste_Total
```

#### **Fatores de Ajuste**

| Fator | Excelente | Boa | Regular | Ruim |
|-------|-----------|-----|---------|------|
| **Sementes** | 1.2 | 1.0 | 0.8 | 0.6 |
| **Fitossanitário** | 1.1 | 1.0 | 0.9 | 0.7 |

**Outros Fatores:**
- **Fertilidade do Solo:** Alta (1.25), Média (1.0), Baixa (0.7)
- **Sistema de Irrigação:** Gotejamento (1.3), Aspersão (1.2), Pivô (1.25), Sequeiro (0.8)
- **Condições Climáticas:** Ótimo (1.15), Bom (1.0), Regular (0.85), Ruim (0.6)
- **Nível de Manejo:** Alto (1.2), Médio (1.0), Baixo (0.8)

#### **Rendimentos Base (kg/ha)**
- **Milho:** 8.000 kg/ha
- **Soja:** 3.200 kg/ha
- **Trigo:** 2.800 kg/ha
- **Arroz:** 7.500 kg/ha
- **Feijão:** 2.000 kg/ha

#### **Faixa de Variação**

```mathematica
Produtividade_Mínima = Produtividade × 0.85
Produtividade_Máxima = Produtividade × 1.15
```

---

### **3. SIMULADOR DE CUSTOS**

#### **Cálculo por Categoria**

```mathematica
Total_Categoria = Σ(Preço_Unitário × Quantidade)
```

#### **Custo por Hectare**

```mathematica
Custo/ha = Custo_Total ÷ Área_Total
```

**Análise:** Agrupamento por categorias de insumos com análise financeira simplificada.

---

### **4. CONTROLE DE PRAGAS**

#### **Dano Potencial**

```mathematica
Dano = Dano_Base × Fator_Infestação × Fator_Estágio × Fator_Climático
```

#### **Fatores de Infestação**
- **Baixo:** 0.3
- **Médio:** 0.7  
- **Alto:** 1.0
- **Severo:** 1.5

#### **Fatores de Estágio Fenológico**
- **Vegetativo:** 0.8
- **Floração:** 1.2
- **Frutificação:** 1.3
- **Maturação:** 0.6

#### **Fatores Climáticos**
- **Seco:** 0.8
- **Úmido:** 1.2
- **Chuvoso:** 1.1
- **Muito Quente:** 1.3

#### **Eficácia do Tratamento**
- **Base:** 85%
- **Com Resistência:** 60%
- **Eficácia Anterior:** 90%

#### **Custos por Hectare (R$)**
- **Inseticida:** R$ 120,00
- **Biológico:** R$ 180,00
- **Sistêmico:** R$ 200,00
- **Fungicida:** R$ 150,00

---

### **5. RISCO CLIMÁTICO**

#### **Análise Probabilística Regional**

**Riscos Base por Região:**

| Região | Seca | Chuva Excessiva | Granizo | Geada | Estresse Térmico |
|---------|------|-----------------|---------|--------|------------------|
| **Centro-Oeste** | 25% | 20% | 15% | - | - |
| **Sul** | 20% | 30% | 35% | 40% | - |
| **Nordeste** | 60% | - | - | - | 45% |

#### **Sensibilidade das Culturas**
- **Soja à Seca:** 0.9
- **Milho ao Granizo:** 0.9
- **Trigo à Chuva Excessiva:** 0.8

#### **Risco Ajustado**

```mathematica
Risco_Final = Risco_Regional × Sensibilidade_Cultura × Fator_Mitigação
```

#### **Fatores de Mitigação**
- **Irrigação por Gotejamento:** Reduz risco de seca em 40%
- **Solo Argiloso:** Reduz risco hídrico em 10%

#### **Risco Total Combinado**

```mathematica
RT = 1 - ∏(1 - Risco_Individual_i)
```

---

### **6. CRESCIMENTO DA CULTURA**

#### **Modelo Sigmoidal de Crescimento**

```mathematica
Crescimento = 1 / (1 + e^(-k × (t - t₀))) × Fatores_Ambientais
```

**Parâmetros:**
- `k` = Taxa de crescimento (10 para modelagem rápida)
- `t` = Tempo normalizado (0-1)
- `t₀` = Ponto de inflexão (0.5)

#### **Características por Cultura**

| Cultura | Ciclo (dias) | Altura Máx. (cm) | Biomassa Máx. (g) |
|---------|--------------|-------------------|-------------------|
| **Soja** | 120 | 80 | 150 |
| **Milho** | 140 | 250 | 400 |
| **Tomate** | 110 | 120 | 200 |

#### **Fatores Ambientais**

```mathematica
Fator_Total = Fator_Temperatura × Fator_Umidade × Fator_Luz × Fator_Solo
```

**Coeficientes dos Fatores:**

| Condição | Temperatura | Umidade | Luminosidade | Solo |
|----------|-------------|---------|--------------|------|
| **Ótima/Ideal** | 1.0 | 1.0 | 1.0 | 1.0 |
| **Adequada/Boa** | 0.9 | 0.9 | 0.8 | 0.8 |
| **Limitante/Baixa** | 0.7 | 0.6 | 0.6 | 0.6 |
| **Crítica/Seca** | 0.4 | 0.3 | - | - |

#### **Estágios Fenológicos**
- **Germinação:** 0-10%
- **Vegetativo:** 10-50%
- **Floração:** 50-80%
- **Frutificação:** 80-100%

---

### **7. FERTILIZAÇÃO**

#### **Necessidade Nutricional**

```mathematica
Necessidade = (Extração_Cultura / Eficiência_Fertilizante) × Fator_Solo × Produtividade_Esperada
```

#### **Extração por Tonelada Produzida (kg/t)**

| Cultura | N | P₂O₅ | K₂O |
|---------|---|------|-----|
| **Soja** | 80 | 15 | 20 |
| **Milho** | 15 | 6 | 4 |
| **Tomate** | 3 | 1 | 4 |

#### **Eficiência dos Fertilizantes**
- **Nitrogênio (N):** 60%
- **Fósforo (P₂O₅):** 30%
- **Potássio (K₂O):** 80%

#### **Fatores de Correção do Solo**

```mathematica
Fator_P = 1.0 + (max(0, 15 - P_solo) / 15) × 0.5
Fator_K = 1.0 + (max(0, 80 - K_solo) / 80) × 0.4
```

#### **Interpretação da Análise de Solo**

| Nutriente | Baixo | Médio | Alto | Unidade |
|-----------|-------|-------|------|---------|
| **Fósforo** | <10 | 10-20 | >20 | mg/dm³ |
| **Potássio** | <50 | 50-120 | >120 | mg/dm³ |

**pH do Solo:**
- **Ácido:** <5.5
- **Adequado:** 5.5-6.5
- **Alcalino:** >6.5

#### **Necessidade de Calagem**

```mathematica
NC = (V₂ - V₁) × CTC × f / 100
```

**Parâmetros:**
- `V₁` = Saturação atual
- `V₂` = Saturação desejada (70%)
- `CTC` = Capacidade de troca catiônica
- `f` = Fator de correção (1.0 para calcário)

#### **Parcelamento de Aplicação**
- **Nitrogênio:** 30% no plantio + 70% em cobertura
- **Fósforo:** 100% no plantio
- **Potássio:** 50% no plantio + 50% em cobertura

---

##  **Interface e Funcionalidades**

### **Dashboard Interativo**
- **Indicadores:** Eficiência de uso da água, rendimento potencial, custos por hectare
- **Visualização:** Gráficos dinâmicos e cards informativos
- **Bibliotecas:** Recharts e Chart.js para visualização de dados

### **Módulos de Gestão**
- **Áreas de Plantio:** Cadastro e gestão de propriedades
- **Registros de Medições:** Histórico de dados coletados
- **Simulações:** Cenários configuráveis e comparativos
- **Relatórios:** Geração automática de documentos técnicos

---

##  **Validação e Resultados**

### **Processo de Validação**
O protótipo foi submetido a simulações que reproduziram diferentes condições agroclimáticas e sistemas de manejo, permitindo:

- **Verificação da Consistência:** Validação matemática dos cálculos implementados
- **Análise de Usabilidade:** Testes de interface e experiência do usuário
- **Cenários Diversos:** Reprodução de condições reais de campo

### **Resultados Obtidos**
- **Ambiente Funcional:** Sistema operacional completo e estável
- **Rigor Técnico:** Implementação fiel aos modelos agronômicos
- **Facilidade de Uso:** Interface intuitiva e acessível
- **Ferramenta de Apoio:** Base sólida para tomada de decisões agronômicas

---

##  **Fundamentação Científica**

### **Bases Teóricas**
1. **Irrigação:** Allen et al. (1998) - FAO Paper 56
2. **Produtividade:** EMBRAPA e dados históricos regionais
3. **Controle de Pragas:** Literatura fitossanitária e limiares de dano econômico
4. **Risco Climático:** Dados históricos INMET e modelos probabilísticos

### **Modelos Matemáticos**
- **Penman-Monteith Simplificado:** Evapotranspiração de referência
- **Funções Sigmoidais:** Crescimento de culturas
- **Rotinas NPK:** Recomendação de adubação
- **Métodos Determinísticos:** Estimativa de produtividade e custos

---

##  **Limitações e Considerações**

### **Limitações Técnicas**
- **Estimativas Baseadas:** Médias históricas regionais
- **Variações Microclimáticas:** Não consideradas no modelo atual
- **Validação Local:** Necessária para dados específicos da propriedade
- **Consulta Agronômica:** Recomendada para decisões críticas

### **Perspectivas Futuras**
- **Integração Externa:** Serviços meteorológicos e APIs especializadas
- **Banco de Dados:** Persistência em servidor para análises avançadas
- **Machine Learning:** Modelos preditivos baseados em histórico
- **Sensoriamento Remoto:** Integração com imagens de satélite

---

##  **Informações do Projeto**

**Versão:** 1.0  
**Desenvolvido por:** @Matz-Turing  
**Tecnologia:** React + TypeScript + Vite + Tailwind CSS  
**Hospedagem:** Vercel  
**Licença:** Projeto acadêmico

---

*Este documento serve como referência técnica completa para o Simulador Agrícola Digital, combinando aspectos de implementação com metodologia científica aplicada à agricultura de precisão.*
