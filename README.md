# brain-fuck-devmedia
Resolução e explicação do desafio do vídeo: https://www.youtube.com/watch?v=DAm7SP8mSfE

## Regras
- Imprimir a frase "Quero uma assinatura da DevMedia!" sem aspas;
- Fazer uso de laço de repetição;
- Fazer uso de até 5 endereços de memória;
- Ser o primeiro a resolver e postar nos comentários.

## O que eu pensei
Primeiro busquei um interpretador e testei, depois eu verifiquei nos comentários por alguma resolução, encontrei uma em que foram usados caracteres todos maiúsculos, dei uma olhada no código e decidi fazer diferente, então começei do zero:

Para pensar no "como fazer" e, de fato, fazer o programa eu decidi usar o Notepad++, gosto muito dele por sua grande capacidade de manipular os textos escritos nele.

Depois de escrever a frase eu pensei "Quais dessas letras se repetem mais vezes?", então escrevi a frase "Quero uma assinatura da DevMedia!" separando cada letra em uma linha. Depois, na frente das letras eu coloquei seus repectivos valores da tabela ASC-II.

Obvio que despois de fazer isso eu não vi padrão nenhum pq não estava organizado, ai pedi o Notepad++ pra organizar as linhas em ordem crescente considerando o número da tabela ASC-II das letras que estavam escritas. Isso resultou no que está no arquivo [analise](analise).

Tinha algumas letras que se repetiam várias vezes, mas o que mais me agradou era que tinha uns grupos de letras próximas, vou exemplificar: "assinatura" (umas das palavras da frase) ela não só tem a letra 'a' repetida três vezes como as letras 's', 'r', 't', 'u' estão próximas umas das outras no alfabeto e, consequentimente, na tabela ASC-II, assim é mais fácil de mudar as letras se eu tiver grupos em posições distantes da tabela e ficar mudando de grupo para pegar letras distantes. Assim eu separei as letras das frases nos seguintes grupos:

1. ' ' (espaço) (posição 32 na tabela ASC-II);
2. 'D', 'M' e 'Q' (posição 68, 77, 81 respectivamente na tabela ASC-II);
3. As demais letras... (posição entre 97 até 118 na tabela ASC-II);
4. '!' (posição 33 na tabela ASC-II);

Como limitaram o uso para 5 endereços e resaltaram a importância do '!' (exclamação), coloquei-o separado no quarto grupo, assim eu usaria os 5 endereços.

*Eu sei, eu sei, eu poderia ter separado o terceiro grupo em dois (entre 'a' e 'i' e outro entre 'm' e 'v') e deixado o '!' no primeiro grupo, mas eu só pensei nisso depois, desculpa.*

## Mãos ao código
Para começar eu posicionei os grupos em seus lugares, usando os endereços de memória da seguinte maneira:
1. Primeiro endereço serve só para fazer os laços de repetição, não será usado para imprimir as letras
2. O Segundo enderço contém o primeiro grupo, ou seja a posição do caracter espaço na tabela ASC-II: **32**
3. O Terceiro endereço é para o segundo grupo, então começa com a posição da primeira letra que irei usar deste grupo, a letra 'Q', ou seja, **81**
4. No quarto endereço fica o terceiro grupo, então recebe a posição da primeira letra que vou usar desse grupo, a letra 'u', ou seja, posição **117**
5. Para o quinto endereço reservei a posição do ponto de exclamação, **33**

Assim eu obtive o seguinte código:
```
// Coloca a posição 32 no segundo endereço
+++
[>++++++++++<-]
>++

// Coloca a posição 81 no terceiro endereço
<++++++++
[>>++++++++++<<-]
>>+

// Coloca a posição 117 no quarto endereço
<<+++++++++++
[>>>++++++++++<<<-]
>>>+++++++

// Coloca a posição 33 no quinto endereço
<<<+++
[>>>>++++++++++<<<<-]
>>>>+++

// Volta para o primeiro endereço, só para não me confundir
<<<<

```

Setadas as posições iniciais de cada grupo em seu respectivos endereços, agora já posso sair imprimindo as letras:
```
>>.               // Letra Q (81)
>.                // Letra u (117)
----------------. // Letra e (101)
+++++++++++++.    // Letra r (114)
---.              // Letra o (111)
<<.               // Espaço em branco (32)
>>++++++.         // Letra u (117)
--------.         // Letra m (109)

```
Agora eu tive uma ideia que vai diminuir meu código, a letra 'a' se repete várias vezes daqui pra frente e a maioria das outras letras estão um tanto quanto distantes do 'a', isso não seria um problema de eu tivesse separado os grupos com mais cautela, mas...
Então eu resolvo fazer o seguinte: Eu pego o endereço do segundo grupo (das letras em maiúsculo e coloco nele a posição da letra 'a'), depois, usarei o endereço do espaço para as letras maiúsculas restantes quando não precisar mais da posição do espaço...
```
// Elevo a terceira memória para 97 (Letra a)
// Essa memória já tinha o valor 81, por isso
// incrementei só 16 na repetição.
<<<++
[>>++++++++<<-]

```
Depois disso voltei a imprimir as letras:
```
>>.               // Letra a (97)
<.                // Espaço em branco (32)
>.                // Letra a (97)
>++++++.          // Letra s (115)
.                 // Letra s (115)
----------.       // Letra i (105)
+++++.            // Letra n (110)
<.                // Letra a (97)
>++++++.          // Letra t (116)
+.                // Letra u (117)
---.              // Letra r (114)
<.                // Letra a (97)
<.                // Espaço em branco (32)
>+++.             // Letra d (100)
---.              // Letra a (97)
<.                // Espaço em branco (32)

```
Lembra que falei sobre usar o endereço do espaço para as letras maiúsculas restantes? Então, é agora:
```
// Elevo a segunda memória para 62 (Letra D)
// Essa memória já tinha o valor 32, por isso
// incrementei só 30 na repetição.
<+++
[>++++++++++<-]

```
Resolvido esses problemas com os grupos, termino de imprimir as letras restantes:
```
>++++++.           // Letra D (68)
>++++.             // Letra e (101)
>++++.             // Letra v (118)
<<+++++++++.       // Letra M (77)
>.                 // Letra e (101)
-.                 // Letra d (100)
+++++.             // Letra i (105)
--------.          // Letra a (97)
>>.                // Exclamação (33)
```

## Tá aí
Terminado o código que imprime a frase "Quero uma assinatura da DevMedia!" na linguagem **Brainfuck** com todas as letras, espaços e caracteres especiais.
O Código completo e comentado esta [aqui](comentado) e código sem comentários e levemente *"apertado"* está [aqui](compactado).
