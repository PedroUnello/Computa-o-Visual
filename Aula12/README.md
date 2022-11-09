> ## Tendo vista os conteúdos passados nesta semana sobre pipeline gráfica, como tal aquilo que foi proposto em sala de aula como atividade semanal, foi realizado, e de mesma forma aqui se encontra, uma pequeno trecho resumido de pesquisas sobre APIs gráficas, contemplando como a pipeline é documentada pelos desenvolvedores da API e quais linguagens de shading (shaders) são suportadas por estas. Concomitânte a isto, pode-se encontrar o resultado da pesquisa abaixo, para as API selecionadas, que foram:

---

<br>

##  OpenGL:


Sobre a pipeline:

> Em discorrer sobre a pipeline da API gráfica é necessário préviamente apresentar que o OpenGL possuí um wiki própria, encontrada em:
> <br><br> _Ref:(https://www.khronos.org/opengl/wiki/Main_Page)_
> <br><br> A documentação da pipeline pode ser encontrada na página (https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview), e conceitualmente, ao que se pode verificar no agregado de informações redigidos pelos autores, é similar ao que fora apresentado em aulas. 
>
>Como tal, a pipeline é um processo com diversas etapas, em muitas, opcionais, que é inicializada uma vez que se tem um programa e objetos que são arrays de vértices préviamente definidos, em uma etapa denominada Vertex Specification, na presença de tais, os vértices são processados e uma etapa chamada Vertex Processing, onde os vértices predefinidos pra renderização podem ser aplicados de um shader para vértices, com operações básicas e arbitrárias per-vertice, após isso é prevista a tesselação, aonde primitivas são aplicadas, conforme necessário, operações de mudanças na topologia, posteriormente é aplicado shaders de geometria, assim em cada e para cada primitiva pode-se aplicar operações para retorno de uma coleção de primitivas, é portanto passado para o pós-processamento dos vertices, começando com uma etapa denominada Transform Feedback, onde a coleção e/ou saída do shader de geometria é engendrado em um buffer de objetos para uso posterior, O que segue na etapa Primitive assembly, montando, baseado nos vértices processados das etapas anteriores, primitivas. 
>
>Isso segue na etapa de Clipping, aonde, definido um volume e/ou perspectiva de visão há uma transformação ou recorte de primitivas em outras ou ademais, para permitir que sejam descartadas posteriormente, o que por sua vez compreende a etapa de Face Culling, onde as primitivas (triângulares), são descartadas para renderização, impedindo que se renderize artefatos que não são visíveis. 
>
> Ao que segue a Rasterização, onde as primitivas não descartadas, da saída nas etapas anteriores, formam fragmentos, ao que correspondem nos pixels finalmente renderizados no frame, diretamente isso desencadeia o processamento de fragmentos, como uma nova etapa, onde serão aplicados os shaders de fragmento (frag shader), possívelmente aplicando ou manipulando valores de depth, stencil e/ou cor. 
>
>Ao fim é pressuposto a etapa de Per-Sample Operations, onde o output do frag shader, é aplicado a um agregado de testes, como Pixel ownership test (overlap de janelas), scissor test (recorte de visão/perspectiva), stencil test (comparação de valores do buffer de stencil), depth test (mesmo do stencil, porem com o buffer de depth), finalizando na operação de color blending, operações lógicas e mascára para remoção de pixels, para engendrar o frame renderizado.

Concomitante a isto, e para ilustrar o que foi antes dito, pode-se verificar a imagem abaixo, também retirada da wiki do OpenGL antes citada:

![Pipeline](https://www.khronos.org/opengl/wiki_opengl/images/RenderingPipeline.png)

_(Ref: https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview)_

---

Em relação as linguagens de shader suportadas pela API, tem-se a GLSL ou OpenGL Shading Language, uma linguagem estruturada similarmente ao C, propícia ao uso em OpenGL por ser criada e estruturada pelos mesmo criadores, a linguagem recebeu adversas atualizações, com a última versão sendo a 4.60, por fim, similarmente ao que foi dito sobre a pipeline, existe uma wiki e página de documentação com referências para a GLSL, tais que podem ser encontradas abaixo, como segue.

- [Página da wiki sobre GLSL](https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language)

- [Página de referência da linguagem](https://registry.khronos.org/OpenGL-Refpages/gl4/index.php)

---

Para exemplicar o funcionamento, aqui são apresentados códigos simples / "Hello World" para,
    
<li> API gráfica OpenGL:
<br><br>

    #include <GL/gl.h>

    void initGL(void)
    {
        glMatrixMode(GL_PROJECTION);
        glLoadIdentity();
        glOrtho(-RESOLUTION_WIDTH/2, RESOLUTION_WIDTH/2, -RESOLUTION_HEIGHT/2, RESOLUTION_HEIGHT/2, 1, -1);
        glMatrixMode(GL_MODELVIEW);
        glLoadIdentity();
    }
    
    void render(void)
    {
        glClear(GL_COLOR_BUFFER_BIT);
        
        glRotatef(1, 0, 0, 1);
        glBegin(GL_TRIANGLES);
            glColor3f(1, 0, 0); glVertex2f( 0,  32); /* Bottom      */
            glColor3f(0, 1, 0); glVertex2f(-32, 0 ); /* Upper Left  */
            glColor3f(0, 0, 1); glVertex2f( 32, 0 ); /* Upper Right */
        glEnd();
    }

_(Ref: https://en.wikiversity.org/wiki/OpenGL/Hello_World)_

<br>

<li> Linguagem de shader GLSL: 
<br><br>        

    #ifdef GL_ES
    precision mediump float;
    #endif

    uniform float u_time;

    void main() {
        gl_FragColor = vec4(1.000,0.057,0.124,1.000);
    }  

_(Ref: https://thebookofshaders.com/02/)_
      
       
---

Finalmente, em citar exemplos de aplicações que utilizam do OpenGL, pode-se atrelar o software [Blender](https://www.blender.org), que possui uma engine estruturada em cima (e com várias chamadas), para o OpenGL, além da game engine [Unity](https://unity.com/pt), mesmo que nesta exista suporte para outras API gráficas, não sendo essencialmente estruturada ou limitada ao OpenGL 


<br>
<br>

---

<br>
<br>

##  Vulkan:


Sobre a pipeline:

> Antes de discorrer sobre a pipeline da API gráfica Vulkan, é coerênte que se apresente a fonte das informações, que aqui tão pouco foi visando abranger fontes dadas como "diretas" aos desenvolvedores, mas sim, apenas que contemplem o assunto pesquisado, portanto, em redigir esse excerto, foi utilizado das informações do site:
>
> _(Ref: https://vulkan-tutorial.com/Drawing_a_triangle/Graphics_pipeline_basics/Introduction)_
>
> Para inicio do processo da pipeline, providos os dados que se quer renderizar, como um agregado a ser processado e transformado em pixels em um tela, a etapa de Vertex Shader roda para cada vértice renderizado na imagem final, resultando nas localizações e parâmetros dos vertices destinados a GPU.
>
> Disso a Tesselização abrange as primitivas dadas nos vértices para gerar outras primitivas, seja da subdivisão da estrutura em detalhes, ou decimação dessa.
>
> Após, é passado o estágio para Geometry Shader, onde a suposição de operações nas primitivas, per vértice, permite manipulação da geometria e primitivas em diferentes geometrias e primitivas.
>
> Posteriomente, a rasterização discretiza as primitivas em fragmentos, contemplando aos pixels que preenchem o buffer do frame, ainda depreendendo de descartar aquilo em que não sentido renderizar, por questões de perspectiva, overlap, etc...
>
> Dado a confecção dos fragmentos, é permissiado a aplicação de shaders de fragmento (frag shader), aplicando operações em cores, luz, profundidade, stencil, mascáras, e mais, antes dos fragmentos serem aplicados em renderização do frame atual.
>
> Tal aplicação compondo, inicialmente, de juntar fragmentos e pixels iguais ou que ocupam igualmente espaços no frame. Dado que o próximo estágio e final para a operação, envolve a composição do frame.

Para ilustrar o que foi antes dito, pode-se verificar a imagem abaixo, também retirada do site anteriormente citado:

![Pipeline](https://vulkan-tutorial.com/images/vulkan_simplified_pipeline.svg)

_(Ref: https://vulkan-tutorial.com/Drawing_a_triangle/Graphics_pipeline_basics/Introduction)_

---

Em relação as linguagens de shader suportadas pela API, sabe-se que é necessário que o código de shader seja especificado e codificado em um formado de bytecode denominado SPIR-V.

Ainda sim, podem e geralmente vai se utilizar outras linguagens na programação de shaders para Vulkan, prévio ao SPIR-V, e por tal via, também é possível dizer, em relação as linguagens de shader suportadas pela API, que tem-se a GLSL ou OpenGL Shading Language (devidamente apresentada na seção da API gráfica OpenGL), além da HLSL ou High-Level Shading Language que é uma linguagem de alto nível oficialmente criada pela Microsoft como a linguagem de shading oficial do DirectX

---

Para exemplicar o funcionamento, aqui são apresentados códigos simples / "Hello World", completamente retirados de diversas fontes (ainda que citadas), para,
    
<li> API gráfica Vulkan:
<br><br>

    Dada a complexidade dos exemplos achados, e coerênte dificuldade de apresentar no espaço delimitado aqui, segue como adendo, apenas um link, para um tutorial de Vulkan similar a um "Hello World".


_(Ref: https://vulkan-tutorial.com/Drawing_a_triangle/Drawing/Rendering_and_presentation)_

<br>

<li> Linguagem de shader GLSL: 
<br><br>        

    #ifdef GL_ES
    precision mediump float;
    #endif

    uniform float u_time;

    void main() {
        gl_FragColor = vec4(1.000,0.057,0.124,1.000);
    }  

_(Ref (retirado de.): https://thebookofshaders.com/02/)_
      

<br>

<li> Linguagem de shader HLSL: 
<br><br>        

    const int L[18]={
        0xad27, 0xa925, 0xED25, 0xa925, 0xaDB7, 0x0000,
        0x85eE, 0x852b, 0xad2b, 0xf92e, 0x712b, 0x51e9, 0x0000,
        0xC3CF, 0xc36f, 0xc326, 0xc360, 0xf3cf
    };
    float4 PS(float2 tex : TEXCOORD0):COLOR0
    {
        float4 output = float4(0,tex.x,tex.y,1);
        int x= (1-tex.x)*16;
        int y= (1-tex.y)*18;
        //no bitwise ioerations on shadermodel 3 yet :(
        int mask=L[y]*2;
        for (int i = 0; i<16&& i <x; i++)
            mask*=0.5;output.r = ( frac(0.5*mask) < 0.1);
        return output;
    }

_(Ref (retirado de.): https://komap.wordpress.com/2009/12/17/hello-world-program-on-hlsl/)_   

---

Frente ao que foi falado anteriormente, e visando citar exemplos de aplicações que utilizam da Vulkan, é passível que se cite aplicações como jogos, a exemplo do [Dota 2](https://www.dota2.com/home), além disso pode ser citado a game engine [Unreal Engine](https://www.unrealengine.com/pt-BR), tendo que esta possui suporte ao Vulkan, mas não se limita ao mesmo, similarmente a outras engines como a [Unity](https://unity.com/pt) e Source 2 (que também possuem suporte, mas não se limitam à).