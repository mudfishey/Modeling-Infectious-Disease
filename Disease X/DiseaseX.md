# Disease X
Sumin Kim

<style>
  .custom-small table {
    font-size: .5em
  }
</style>

# Introduction

Disease X is a respiratory virus disease that is rapidly spreading. The
outbreak has begun in Berlin, and is beginning in Ho Chi Minh City. The
WHO wants to know if disease X will be contained and what can be done to
halt the disease becoming the epidemic and minimize the severity of the
disease.

This report proposes the simplified mathematical model of Disease X to
investigate its infection dynamics and explore the potential control
measures that can halt the epidemic and minimize the severity of the
disease.

# Background

> [!NOTE]
>
> **Transmission dynamics:**
>
> - Urban areas of Berlin saw higher transmission rates (0.75 per day).
>
> - Rural areas had 30% of the urban transmission rate.
>
> - In Ho Chi Minh City, ~60% of the population is lower risk with
>   similar transmission differences.
>
> **Current infection status in Ho Chi Minh City:**
>
> - 5 suspected cases, 2 confirmed cases in a 3-million population.
>
> - No latent exposed individuals assumed.
>
> **Disease progression & infectiousness:**
>
> - 10% of exposed individuals become infectious before symptoms.
>
> - Latency period: ~3 days before symptoms appear.
>
> - Severe cases remain infectious for ~8 days before recovery with
>   assumed long-term immunity.
>
> - Diagnosed individuals recover in ~5 days with immunity.
>
> **Diagnosis & treatment:**
>
> - Antivirals reduce infectiousness to 38% of undiagnosed individuals.
> - Diagnosis rate in Berlin: 1/4 per day; Ho Chi Minh City estimated to
>   be 1/3 per day.
>
> **Mortality:**
>
> - Estimated at 0.006 per day, same for both diagnosed and undiagnosed
>   individuals.

## Model 1 - Basic model with quarantine

This is the basic compartmental model scheme of Disease X, constructed
with the given information.

![Fig 1. Model Scheme of Disease X without
quarantine](Fig%201.%20Model%20Scheme%20of%20Disease%20X%20without%20quarantine.png)

<style>
table {
font-size: 10px;
}
</style>

| Symbol   | Definition                                            |
|----------|-------------------------------------------------------|
| $S_{HQ}$ | Susceptible in high-risk population, quarantined      |
| $S_H$    | Susceptible in high-risk population (not quarantined) |
| $S_{LQ}$ | Susceptible in low-risk population, quarantined       |
| $S_L$    | Susceptible in low-risk population (not quarantined)  |
| $E_Q$    | Exposed, quarantined                                  |
| $E_{NQ}$ | Exposed, not quarantined                              |
| $I_Q$    | Infected, quarantined                                 |
| $I_U$    | Infected, not quarantined                             |
| $I_D$    | Infected, diagnosed (quarantined at this point)       |
| $D$      | Death                                                 |
| $R$      | Recovered                                             |

| Parameter | Definition | Value |
|----|----|----|
| $\alpha$ | Average time until immunity is lost | 180 days ($\text{rate} = \frac{1}{180}$ day⁻¹) |
| $k$ | Baseline daily number of contacts per capita | 10 (day⁻¹) |
| $b_0$ | Transmission probability per contact in high-risk population | 0.075 |
| $b_1$ | Transmission probability per contact in low-risk population | $0.075 \times \frac{1}{3}$ |
| $p$ | Progression rate from exposed to infectious | $\frac{1}{3}$ (day⁻¹) |
| $v_0$ | Per capita recovery rate **with** treatment | $\frac{1}{5}$ (day⁻¹) |
| $v_1$ | Per capita recovery rate **without** treatment | $\frac{1}{8}$ (day⁻¹) |
| $m$ | Per capita death rate | 0.006 (6 out of 1000 dies daily) |
| $w$ | Daily rate of diagnosis and access to proper treatment | $\frac{1}{3}$ (day⁻¹) |
| $r_Q$ | Average quarantine duration for susceptible, unexposed and uninfected individuals | 14 days ($\text{rate} = \frac{1}{14}$ day⁻¹) |
| $q$ | Fraction of uninfected & unexposed individuals being quarantined | 0 to 1 |

We consider two distinct populations: a high-risk population ($S_H$) and
a low-risk population ($S_L$), which, for simplicity, are referred to as
urban and rural populations, respectively. A portion of individuals in
both $S_H$ and $S_L$ who have not yet been exposed to the virus are
quarantined, represented mathematically as
$kq(1 - b)I_\text{total} / N_0$.

Those who are not quarantined are susceptible to virus exposure. Upon
exposure, individuals in $S_H$ and $S_L$ transition to either a
quarantined exposed state ($E_Q$) or a non-quarantined exposed state
($E_{NQ}$). Regardless of quarantine status, once exposed, it takes a
period $p$ for individuals to become symptomatic and fully infectious.
At that point, $E_Q$ and $E_{NQ}$ progress to infectious states—either
quarantined ($I_Q$) or non-quarantined ($I_{NQ}$). From there,
individuals face three possible outcomes: death (at a rate $m$),
recovery without treatment (at a rate $v_1$), or proper diagnosis and
treatment (at a rate $w$). Once properly treated, individuals recover at
a faster rate $v_0$.

After diagnosis and proper treatment (denoted as $I_d$), individuals
either recover more quickly (at rate $v_0$) or still face a risk of
death (at rate $m$).

This conceptual framework is represented mathematically by the equations
provided below.

$$
\small
\begin{align*}
\text{Total infectious:} \quad & I_{\text{total}} = 0.10 \cdot E_{\text{NQ}} + I_{\text{U}} + 0.38 \cdot I_{\text{D}} \\[1em]
\textbf{1.}  &\quad \frac{dS_{\text{HQ}}}{dt} = k(1 - b_0)q \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - r_Q S_{\text{HQ}} \\\textbf{2.}  &\quad \frac{dS_{\text{H}}}{dt} = r_Q S_{\text{HQ}} - kq(1 - b_0) \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - k(1 - q)b_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - kqb_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} \\\textbf{3.}  &\quad \frac{dS_{\text{LQ}}}{dt} = k(1 - b_1)q \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - r_Q S_{\text{LQ}} \\\textbf{4.}  &\quad \frac{dS_{\text{L}}}{dt} = r_Q S_{\text{LQ}} - kq(1 - b_1) \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - k(1 - q)b_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - kqb_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} \\\textbf{5.}  &\quad \frac{dE_{\text{Q}}}{dt} = kqb_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} + kqb_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - p E_{\text{Q}} \\\textbf{6.}  &\quad \frac{dE_{\text{NQ}}}{dt} = k(1 - q)b_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} + k(1 - q)b_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - p E_{\text{NQ}} \\\textbf{7.}  &\quad \frac{dI_{\text{Q}}}{dt} = p E_{\text{Q}} - I_{\text{Q}}(m + w_1 + v_0) \\\textbf{8.}  &\quad \frac{dI_{\text{U}}}{dt} = p E_{\text{NQ}} - I_{\text{U}}(m + w_2 + v_0) \\\textbf{9.}  &\quad \frac{dI_{\text{D}}}{dt} = w_1 I_{\text{Q}} + w_2 I_{\text{U}} - m I_{\text{D}} - v_1 I_{\text{D}} \\\textbf{10.} &\quad \frac{dD}{dt} = m(I_{\text{Q}} + I_{\text{U}} + I_{\text{D}}) \\\textbf{11.} &\quad \frac{dR}{dt} = v_0(I_{\text{Q}} + I_{\text{U}}) + v_1 I_{\text{D}}\end{align*}
$$

The assumptions made to construct this model are as follows:

- Metapopulation effects are considered negligible.
- Interactions between the urban and rural populations are assumed to be
  negligible.
- Seasonal variation in transmission is not included.
- Only individuals who are susceptible and not yet exposed are subject
  to quarantine.
- Once individuals are exposed, they may either be quarantined ($E_Q$)
  or not quarantined ($E_{NQ}$). Regardless of quarantine status, the
  diagnosis rate remains the same and is denoted by $w$.
- If diagnosed, individuals are assumed to receive proper treatment,
  which leads to faster recovery at rate $v_0$, compared to untreated
  recovery at rate $v_1$.
- The model operates under a batch-processing structure, meaning there
  is no inflow of new individuals (e.g., through births). Consequently,
  most individuals will eventually transition to either the deceased
  ($D$) or recovered ($R$) compartments, with minimal long-term
  retention in intermediate states such as exposed ($E$) or infectious
  ($I$).

### Research Question 1. What is the expected dynamics of diseases X?

Without any quarantine or other preventative measure, the disease shows
the following dynamic for 1.5 years like below.

- Quarantine level: 0

- Population: 3000000000 (3million)

- Duration: 1.5 year

![](DiseaseX_files/figure-commonmark/unnamed-chunk-2-1.png)

In the absence of any intervention (i.e., with zero quarantine), the
model indicates that the epidemic begins around Day 200 and reaches
equilibrium by approximately Day 280. This suggests that, without
external control measures, the disease naturally subsides within about
1.5 years.

In the 90-day projection, the epidemic appears inactive, as the graph
lines remain nearly flat. However, changes are indeed occurring—though
minimal and not visually prominent due to the large total population
size (3 million). For example, when infections increase from 34 to 45
cases, the change is numerically small and difficult to detect on a
population-scale graph. This slow initial progression is expected, given
that the outbreak starts with only 5 diagnosed cases in a large
population.

This pattern reflects real-world epidemics, such as the early stages of
the COVID-19 pandemic. While the outbreak began in China and spread
rapidly to neighboring countries like South Korea and Japan, it took
several months—approximately six months to a year—for significant
transmission to occur in regions like Europe and the United States. A
detailed timeline of the COVID-19 pandemic is available here: [Timeline
of the COVID-19
pandemic](https://en.wikipedia.org/wiki/Timeline_of_the_COVID-19_pandemic).

### Research Question 2. What is the effectiveness of quarantine?

Let’s introduce the quarantine and see if it has an impact.

- Quarantine level: 25%

- Population: 3000000000 (3million)

- Duration: 1.5 year

![](DiseaseX_files/figure-commonmark/unnamed-chunk-4-1.png)

When quarantine measures are introduced, the number of infected
individuals decreases significantly. Additionally, the number of people
who remain uninfected by the end of the epidemic is higher compared to
the scenario with no quarantine. This indicates that quarantine helps
prevent exposure, allowing a greater proportion of the population to
remain susceptible but uninfected. Consequently, both the number of
deaths and recoveries decline, as fewer people contract the disease
overall.

This raises an important question: what level of quarantine is most
effective? To answer this, a sensitivity analysis is recommended to
evaluate how varying degrees of quarantine influence outcomes such as
total deaths. This approach can help identify an optimal balance between
public health impact and the extent of intervention.

### Research Question 3. Quarantine level sensitivity analysis

![](DiseaseX_files/figure-commonmark/unnamed-chunk-5-1.png)

The sensitivity analysis indicates that a quarantine level of
approximately 39% is the most effective in minimizing total deaths. This
suggests that quarantine, on its own, can be sufficient to prevent
Disease X from progressing into a widespread epidemic and significantly
reduce mortality. However, implementing and enforcing quarantine
measures at this scale can be logistically and socially challenging.

> Therefore, we recommend adopting a quarantine level of around 38% in
> combination with additional preventive strategies—such as
> mask-wearing, vaccination, and social distancing—to further reduce
> population exposure and enhance overall outbreak control.

## Model 2 - lost immunity after 180 days

This is the compartmental model scheme of Disease X, where the immunity
is lost after 180days.

![Fig 2. Model Scheme of Disease X with lost immunity after 180
days](Fig%202.%20Model%20Scheme%20of%20Disease%20X%20with%20lost%20immunity%20after%20180%20days.png)

$$
\small
\begin{align*}
\text{Total infectious:} \quad & I_{\text{total}} = 0.10 \cdot E_{\text{NQ}} + I_{\text{U}} + 0.38 \cdot I_{\text{D}} \\[1em]
\textbf{1.}  &\quad \frac{dS_{\text{HQ}}}{dt} = k(1 - b_0)q \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - r_Q S_{\text{HQ}} \\
\textbf{2.}  &\quad \frac{dS_{\text{H}}}{dt} = \alpha R + r_Q S_{\text{HQ}} - kq(1 - b_0) \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - k(1 - q)b_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} - kqb_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} \\
\textbf{3.}  &\quad \frac{dS_{\text{LQ}}}{dt} = k(1 - b_1)q \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - r_Q S_{\text{LQ}} \\
\textbf{4.}  &\quad \frac{dS_{\text{L}}}{dt} = \alpha R + r_Q S_{\text{LQ}} - kq(1 - b_1) \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - k(1 - q)b_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - kqb_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} \\
\textbf{5.}  &\quad \frac{dE_{\text{Q}}}{dt} = kqb_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} + kqb_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - p E_{\text{Q}} \\
\textbf{6.}  &\quad \frac{dE_{\text{NQ}}}{dt} = k(1 - q)b_0 \cdot \frac{S_{\text{H}} I_{\text{total}}}{N_0} + k(1 - q)b_1 \cdot \frac{S_{\text{L}} I_{\text{total}}}{N_0} - p E_{\text{NQ}} \\
\textbf{7.}  &\quad \frac{dI_{\text{Q}}}{dt} = p E_{\text{Q}} - I_{\text{Q}}(m + w_1 + v_0) \\
\textbf{8.}  &\quad \frac{dI_{\text{U}}}{dt} = p E_{\text{NQ}} - I_{\text{U}}(m + w_2 + v_0) \\
\textbf{9.}  &\quad \frac{dI_{\text{D}}}{dt} = w_1 I_{\text{Q}} + w_2 I_{\text{U}} - m I_{\text{D}} - v_1 I_{\text{D}} \\
\textbf{10.} &\quad \frac{dD}{dt} = m(I_{\text{Q}} + I_{\text{U}} + I_{\text{D}}) \\
\textbf{11.} &\quad \frac{dR}{dt} = v_0(I_{\text{Q}} + I_{\text{U}}) + v_1 I_{\text{D}} - 2\alpha R
\end{align*}
$$

![](DiseaseX_files/figure-commonmark/unnamed-chunk-6-1.png)

When immunity wanes after 180 days and the population becomes
susceptible again, the epidemic begins earlier and, without any
quarantine measures, it does not end naturally as it does in model 1.
The death toll continues to rise, and equilibrium—the end of the
epidemic—is not reached even after 800 days.

With a quarantine level of 35%, equilibrium is eventually achieved, and
the overall size of the epidemic is smaller. However, the death toll
still rises steadily in a similar pattern.

Introducing quarantine in this scenario has a significant impact,
greatly reducing the damage caused by the epidemic.

# Conclusions

Introducing quarantine has a significant impact in preventing the spread
of Disease X, as demonstrated by all models across different scenarios.
Based on sensitivity analysis, we recommend adopting a quarantine level
of approximately 38%, combined with additional preventive measures—such
as mask-wearing, vaccination, and social distancing—to further reduce
population exposure and enhance overall outbreak control.
