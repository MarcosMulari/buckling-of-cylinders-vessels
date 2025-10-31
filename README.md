# Buckling of Thin Cylindrical Shells

Este projeto implementa as fórmulas de Ventsel para análise de flambagem (buckling) de cascas cilíndricas finas sob pressão externa.

## Descrição

O código calcula a pressão crítica de flambagem para cascas cilíndricas considerando três regimes distintos baseados no comprimento:

- **Regime Curto** (L < 2.5√(Rt)): Para cilindros curtos onde o comprimento tem forte influência
- **Regime Intermediário** (2.5√(Rt) ≤ L ≤ 3.35R√(R/t)): Regime de transição
- **Regime Longo** (L > 3.35R√(R/t)): Para cilindros longos onde a pressão crítica independe do comprimento

## Estrutura do Projeto

```
buckling-thin-plates/
├── ventsel-thin-plates.ipynb    # Notebook principal com implementação e análises
├── figs_ventsel/                # Diretório de saída para figuras geradas
└── README.md                    # Este arquivo
```

## Funcionalidades Principais

### Função `p_cr_ventsel()`

Calcula a pressão crítica de flambagem com os seguintes parâmetros:

- **E**: Módulo de elasticidade (Pa)
- **t**: Espessura da casca (m)
- **R**: Raio da casca (m)
- **L**: Comprimento da casca (m)
- **nu**: Coeficiente de Poisson
- **regime**: "auto", "short", "intermediate" ou "long"
- **cond_contorno**: Condições de contorno ("simples_apoio" ou "pino_engaste")

### Fórmulas Implementadas

#### Critérios de Transição entre Regimes

```
L_crítico_curto = 2.5 × √(R × t)
L_crítico_longo = 3.35 × R × √(R / t)
```

#### Regime Longo (L > 3.35R√(R/t))

```
p_cr = fator_cc × 0.275 × E × (t/R)³
```

Independe do comprimento L. A pressão crítica permanece constante.

#### Regime Intermediário (2.5√(Rt) ≤ L ≤ 3.35R√(R/t))

```
p_cr = fator_cc × 0.855 × α × (E × t²) / (R × L × (1 - ν²)^(3/4)) × √(t/R)
```

onde α é interpolado de uma tabela em função de R/t (valores típicos: 0.4 a 0.7).

#### Regime Curto (L < 2.5√(Rt))

```
p_cr = fator_cc × α₁ × E × t² / (R × L)
```

onde:
- α₁ = 1.8 (pressão no contorno todo, padrão)
- α₁ = 3.6 (pressão alternativa)

#### Fator de Condição de Contorno (fator_cc)

- **Simples apoio**: fator_cc = 1.0
- **Pino-engaste**: fator_cc = 0.6

## Parâmetros Padrão

```python
E  = 70e9      # Pa (Alumínio)
nu = 0.33      # Coeficiente de Poisson
t  = 0.001     # m (1 mm)
R  = 0.15      # m (150 mm)
```

## Saídas Geradas

O notebook gera os seguintes arquivos na pasta `figs_ventsel/`:

- `pcr_vs_L_R_fix_2regimes.png` - Gráfico da pressão crítica vs comprimento
- `pcr_vs_L_R_fix_2regimes.pdf` - Versão PDF do gráfico

## Requisitos

```python
numpy
matplotlib
```

## Como Usar

1. Abra o notebook `ventsel-thin-plates.ipynb`
2. Execute as células sequencialmente
3. Ajuste os parâmetros conforme necessário (E, t, R, L, nu)
4. Os gráficos serão salvos automaticamente em `figs_ventsel/`

## Referências

Baseado nas equações de Ventsel para análise de estabilidade de cascas cilíndricas finas.

## Autor

Desenvolvido para análise estrutural de cascas cilíndricas sob pressão externa.

## Licença

Este projeto é fornecido como está para fins educacionais e de pesquisa.
