# Aula 14 - Iluminação

> Como na aula dessa semana, foi estudado sobre iluminação, com equações matemáticas que formulam o visual, quando renderizado, de um objeto iluminado, mas apenas naquilo que diz respeito a objetos em representações realistas. Foi feito uma pesquisa sobre metodologias para renderização de objetos em forma não realista, como tal, segue um agregado de conhecimentos sobre técnicas NPR (non-photorealistic rendering), com ênfase na técnica de toon/cel shading.

---

Primeiramente, em relação a técnica toon shading, entende-se que está funciona por renderizar os objetos com cores constantes com os limites delineados, em vez dos grandientes definidos pelas outras técnicas de shading, já que o objetivo é diminuir a variação no shading. Definindo melhor, a técnica de toon shading envolve 3 componentes, a componente difusa, que precisa ser representada por apenas dois valores por cor do material, uma para regiões com brilho e outra para regiões sem brilho, a componente specular, aonde os destaques pela luz devem ser representadas por uma única cor, quando a intensidade da luz for suficiente, e por fim um outline/borda fortemente destacada como uma cor escura. 

Para a componente difusa, o conceito é baseado em substituir valores arbitrários de cores (de uma componente difusa das técnicas realisticas de rendering), para 2 cores complementares, uma para regiões com pouca luz e uma para regiões com luz suficiente, ou até outra dinâmica de valores, não sendo limitado a luz, assim, poderia ser feito a limiarização dos valores do gradiente de cor da componente difusa "realistica", em outros valores arbitráriamente definidos, em um processo que pode ser explicado na troca da função/curva de valores continuos de cor, para uma função step, que limiariza e mapeia todos os valores anteriores em apenas 2 valores discretos.

Para a componente especular, de maneira muito correlatada a componente anterior, é suposto de aplicar um limiar ou função step da componente especular dos outros modelos de shader, porém, novamente de maneira similar a componente difusa, é suposto a terem dois valores apenas, deve aplicar destaque ou não se deve aplicar destaque, claro em detrimento do gradiente outrora feito para os modelos realisticos.

Por fim, para a componente de outline, entendida como uma silhueta do objeto, deve-se primeiramente achar os pontos inclusos na silhueta do objeto, para tal, pode-se utilizar do vetor normal da superfície N, e do vetor view, dado por V, assim, no dot product de N e V, sabe-se quanto da superfície do objeto é vísivel no momento, dado que um valor negativo de N . V está invisível, e do contrário, é visível, assim valores que estejam próximos a zero e por tal, próximos de valores negativos, deve ser marcados com a cor da silhueta, por estarem no limiar da parte visível e não-visível do objeto. 

> Porém utilizar funções de step de tal forma causa uma grande flutuação de valores, já que a mínima diferença de um valor para outro próximo, pode causar uma mudança do valor definido no limiar, tal que pode ser brusca, assim, para lidar com o aliasing que provavelmente irá ser criado, pode-se utilizar de uma trânsição mais suave de valores, ao que infere uma estratégia, de utilizar um vetor de valores de transição, similar a um gradiente, porém não com tanta força quanto os shaders realisticos. 

Já em questão da técnica de Cel shading, dado que o nome Cel vem do material outrora utilizado para desenho de animações, Celuloide (já que a técnica computa imagens de maneira similar a um desenho feito a mão), similarmente ao Toon shading, os gradientes utilizados nos shadings convencionais são substituidos por blocos de cores e sombramento, alterando a maneira com que a luz decaí pelos objetos.

De mais a mais, entende-se que as diferenças entre Toon shading e Gouraud ou Phong, está na maneira como o decaimento da luz sob um objeto é tratado, aonde Gouraud calcula a intensidade da luz ou sombreamento por interpolar as cores, eliminando descontinuidades bruscas de intensidade (enquanto não elimina qualquer mudança), e trabalhando com a superfície, no vertex shader, Phong trabalha interpolando o vetor normal, pela superfície (normal da superfície), pegando os valores do vetor normal de um determinado ponto, e calculando a intensidade pela interpolação deste e seus consequentes, assim trabalhando no fragment shader. Assim os dois funcionam de maneira similar, interpolando e definindo, neste cálculo um gradiente na intensidade e sombreamento, o Toon shading, porém, trabalha definindo limiares e blocos restritos de cores/intensidades, assim não é necessário um cálculo complexo para definição dos valores e implantação da técnica, apenas necessitanto, como antes discutido, de valores máximo mínimo (função step), para componentes difusa e especular (além da deliniação da borda/silhueta).

---

Dado a formalização dos conceitos, segue uma seção com imagens, ilustrando o que fora trabalho e/ou aprendido:

> _Até a experimentação, alguns tópicos abaixo, todas as imagens que seguem foram retiradas da internet, assim será colocado apenas a referência/link da arte._

<li> Imagens comparando Gouraud/Phong shading:

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRAknOFfQZnDbcH7JS4ZT93Pg2avkUjl1AA-g&usqp=CAU) <br>
_Ref(http://evasion.imag.fr/Enseignement/cours/2012/SIA/Shaders/tp1_en.html)_

![Simples modelo demonstrando diferença entre shaders Ghourad e Phong](https://qph.cf2.quoracdn.net/main-qimg-c509813e14a76deec671cf86ab3e1c6a.webp) <br>
_Ref(https://www.quora.com/What-is-an-explanation-of-the-Gouraud-shading-and-phong-shading-models-in-simple-form)_

<li> Exemplos de Cel Shading (na internet):

![Zelda - BotW](https://cdnb.artstation.com/p/assets/images/images/038/437/823/large/digital-frontiers-cemu-2021-04-21-23-05-29-75n.jpg?1623097289) <br>
_Ref(https://www.artstation.com/artwork/nYexKo)_

![Guilty Gear - Strive](https://www.guiltygear.com/ggst/en/wordpress/wp-content/themes/ggst/img/top/mode_slide03_thumb.jpg) <br>
_Ref(https://www.guiltygear.com/ggst/en/wordpress/wp-content/themes/ggst/img/top/mode_slide03_thumb.jpg)_

<li> Exemplos de Toon Shading (na internet):

![Game - DragonBall Fighters Z](https://cdn.cloudflare.steamstatic.com/steam/apps/678950/ss_3d48baf2e7aaf087afbab1c4268637df528eb2c8.600x338.jpg?t=1653936849) <br>
_Ref(https://cdn.cloudflare.steamstatic.com/steam/apps/678950/ss_3d48baf2e7aaf087afbab1c4268637df528eb2c8.600x338.jpg?t=1653936849)_

![Game - Persona 5 Royal](https://sm.ign.com/ign_br/news/e/everything/everything-announced-during-todays-nintendo-direct-mini-incl_ny2h.jpg) <br>
_Ref(https://sm.ign.com/ign_br/news/e/everything/everything-announced-during-todays-nintendo-direct-mini-incl_ny2h.jpg)_

<li> Por fim, segue imagens de experimentação com o Toon shading, para tal, foi utilizado o programa ZBrush para carregar os objetos (que estavam disponiveis para pré-carregar no programa e não são de autoria, ou feitos para este trabalho), e o programa Adobe Substance Painter para aplicar o shader:

_Mesh original retirada do programa ZBrush, aqui apenas utilizada para teste, pela complexidade do modelo._

![Mesh original - Earthquake](https://github.com/PedroUnello/Computacao-Visual/blob/main/Aula14/BaseMeshKoteInitoffEarthquake.png) <br>
_Ref(https://pixologic.com)_

_Mesh exportada do ZBrush, aqui aplicada de um shader para toon shading no programa Adobe Substance Painter 2022, nota-se que as cores e materiais do modelo original foram trocadas, pela simplicidade de utiliza o modelo, em vez de exportar as cores originais como texturas, etc..._

![Toon Shader Aplicado - Earthquake](https://github.com/PedroUnello/Computacao-Visual/blob/main/Aula14/EarthquakeToonShader.png) <br>
_Ref(https://www.adobe.com/products/substance3d-painter.html)_

_Outra mesh retirada do ZBrush, novamente aqui apenas utilizada para objetivos de teste._

![Mesh original - Estatua](https://github.com/PedroUnello/Computacao-Visual/blob/main/Aula14/BaseMeshStatue.png) <br>
_Ref(https://pixologic.com)_

_Mesh anterior exportada para o Adobe Substance Painter 2022, novamente para aplicar o toon shading._

![Toon Shader Aplicado - Estatua](https://github.com/PedroUnello/Computacao-Visual/blob/main/Aula14/StatueToonShader.png) <br>
_Ref(https://www.adobe.com/products/substance3d-painter.html)_

---

_Referências da pesquisa:_ 

<li> https://developer.download.nvidia.com/CgTutorial/cg_tutorial_chapter09.html
<li> https://www.inf.pucrs.br/~smusse/FCG/PDF_2017_1/Iluminacao.pdf
<li> https://www.haroldserrano.com/blog/what-is-the-difference-between-gouraud-and-phong-shading
<li> https://www.adobe.com/uk/creativecloud/animation/discover/cel-shading.html#:~:text=Cel%20shading%20is%20a%20computer,falls%20across%20a%203D%20model.



