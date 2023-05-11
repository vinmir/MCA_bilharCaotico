# MCA_bilharCaotico
Considere a seguinte situação: em uma mesa de bilhar no formato elíptico ou circular, posicione uma bola a uma dada distância do centroide da figura. Em seguida, aplique um impulso vertical à bola. Qual será a trajetória da bola como função do tempo? Supondo que as reflexões sejam especulares (ou seja, parecidas com a luz), desprezando perdas de energia, a bola poderá ocupar qualquer posição da mesa?

O objetivo deste projeto foi estudar a dinâmica caótica desse tipo de sistema e como ela depende do contorno da mesa. Analiticamente, o projeto esteve associado ao estudo de caos, integrabilidade, espaço de fase, mapas e o teorema KAM da mecânica clássica. Computacionalmente, este projeto esteve diretamente envolvido com a utilização de técnicas numéricas em SciPy para parametrizar contornos dados por curvas implícitas, visualização com Matplotlib, tratamento de dados com Numpy e Numba para otimização de execução.

# Referências
+ Kibble: Classical Mechanics
+ Karl Heinz Hoffmann: Computational Statistical Physics

Para mais informações, consulte os comentários nos notebooks `.ipynb`.

# Imagens

## Contorno circular

Equação implícita: $x^2+y^2=1$

Condições iniciais: $\textbf{r} = (x_0,0)$ e $\textbf{v} = \frac{d\mathbf{r}}{dt} = (0,1)$

Existem posições distâncias iniciais da origem que geram circuitos fechados, isto é, periódicos, como ilustrado nas imagens abaixo:

![xy_N_5](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/c6b09819-8869-49bc-b379-88254389246f)
![xy_N_6](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/fb1056e5-6374-4f2d-af71-3830c5bcf9ab)
![xy_N_9](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/93a71dab-db47-4148-aa13-b3b41ef8d1bd)

As posições iniciais são facilmente determinadas por álgebra modular e geometria plana (consulte o pdf `bilhar.pdf` para mais informações), resultando na forma geral

$$\alpha = \pi\frac{m}{n},\ x_0 = \cos(\alpha),$$

onde $n,m \in \mathbb{Z}$, com $n>2m$ e $n,m\ge1$ e $\alpha$ é o ângulo descrito an imagem abaixo:

![bilhar_eliptico](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/4fee528c-d26b-4dc4-8164-d499110bfa93)

Embora a imagem se refira ao bilhar elíptico, vale para todos os contornos explorados neste projeto. A quantidade $S$ da imagem define o comprimento de arco no ponto da colisão da bola de bilhar, e $p\equiv \cos(\alpha)$ é a coordenda de projeção unitária sobre a reta tangente à curva no ponto de impacto. Estas duas quantidades, $S,p$, são suficientes para descrever todo e qualquer impacto, permitindo o estudo da seção de fase de Poincaré (eixo vertical representa $p$ e o horizontal $S$, normalizado pelo comprimento total do contorno do bilhar):

![fase_N_5](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/3a3d957f-2d9e-47f8-873b-bf2449782411)
![fase_N_6](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/1912b27c-bae3-43f4-8252-4b6054400b90)
![fase_N_9](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/425424d5-25be-4ad6-8cb1-f61bce53b6bd)

Os pontops fixos no espaço de fase são típicos de mapas logísticos.

Caso as condições iniciais não reflitam os casos de órbita fechada, embora $p$ permaneça fixo no espaço de fase (decorrente da conservação de momento angular), agora *todo* ponto de impacto é permitido. Para tempos longos, isso significa que a bola de bilhar ocupa todo o espaço da mesa com $r>x_0$, como ilustrado abaixo:

![xy_x0_0 52](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/e7ae6de2-d63b-4b15-ae90-d29347536d84)
![fase_x0_0 52](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/5f31d2f0-2208-418e-aa6e-42e7f2591588)

## Contorno elíptico

Equação implícita: $x^2/a^2 + y^2/b^2 = 1$, $a=1.5, b=1.3$

Condições iniciais: $\textbf{r} = (x_0,0)$ e $\textbf{v} = (0,1)$

Diferentemente do caso circular, não há órbitas fechadas, mas sim *confinadas* em três regiões distintas, dependentes dos focos ($x\approx\pm0.75$):
1. $|x_0| < 0.75$: seção de Poincaré circular
2. $|x_0| \approx 0.75$: separatriz do espaço de fase, curva fechada
3. $|x_0| > 0.75$: seção de Poincaré hiperbólica


![xy_x0_0 62](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/dd340e5c-a4ec-4108-a548-b6b7d8af94ea)
![xy_x0_0 75](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/d2645b6d-fb30-47dd-ac90-303af9e3dd4a)
![xy_x0_1 14](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/67d40f8e-46a7-494d-abd6-0ef6450df512)

A visualização das seções de Poincaré para diferentes condições iniciais gera o seguinte espaço de fase, perfeitamente análogo ao sistema composto pelo pêndulo simples (ref: Kibble):

![bilhar_elíptico](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/2778bf28-9566-469f-a390-b076b36c057a)

## Deformação do contorno elíptico

Imposição do caos: falta de integrabilidade e previsibilidade da trajetória da bola, quebra da simetria do espaço de fase.

Equação implícita: $x^2/a^2 + y^2/b^2 = 1 - \epsilon \sin(x)$, $a=1.5, b=1.3$, $\epsilon$ variável.

Esta equação não pode ser parametrizada anaiticamente como os outros dois contornos. Uma alternativa para a aparametrização numérica é o uso do vetor gradiente: o vetor perpendicular ao gradiente é tangente à curva, o que significa que, dado um ponto inicial, um vetor velocidade generalizado é obtenível, permitindo evoluir a curva fechada pelo método de Euler. Estes passos estão docuemntados nos notebooks apropriados.

![xy_eps_0 300](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/82d8dc97-a155-40a3-9146-df403316e973)

Repare no formato oval da elipse dada a deformação. Independentemente de $x_0$, a bola percorre toda a região da mesa de bilhar.

O espaço de fase das seções de Poincaré sofre deformações graudais com $\epsilon$:


![bilhar_eps=0 005](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/047b106c-f839-44fd-bb11-1cd6a6fb2ca0)
![bilhar_eps=0 3](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/2c581362-850e-4dd2-b928-6b5f3150e115)
![bilhar_eps=1](https://github.com/vinmir/MCA_bilharCaotico/assets/133194350/09922454-ffe6-4215-8f65-72dc7bd17b31)




