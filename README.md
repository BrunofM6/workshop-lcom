# Workshop LCOM
## Tópicos principais:
Programação em C é frágil (e mais difícil do que parece), é fácil cometer erros e fazer bugs, sendo assim os tópicos fundamentais são:
- máscaras; (selecionar bits) -- importante
- apontadores;
- enumerados;
- union;
- struct;
- extend.

System calls:
- sysoutb() -> para escrever
- utils_sys_inb() (alteração de sysinb()) -> para ler

Programação em C é fundamental, faz muito do que usa a mais alto nível.

### Clock Lab2:
#### Máscaras:
Registro mínimo tem 8 bits, um byte. Em tudo o que é possível poupar memória faz-se, um só byte pode ter várias partes que se traduzem em informações diferentes. Se preciso de um só bit, não vou usar 8, vou juntar com outras partes de informação complementares para 8 bits.
Usa-se máscaras para selecionar os bits, por isso são muito importantes. Operações fundamentais:
- and, or, xor, not...
- shifts. (direita (>>) e esquerda (<<))
Exemplo:
RGB com 16 bits:
R R R R R G G G G G G B B B B B
uint16_t cor;
selecionar azul -> azul = (cor & 0x001F)
selecionar verde -> verde = ((cor & 0x0E70) >> 5) 
selecionar vermelho -> vermelho = ((cor & 0xF800) >> 11)

#### Macros:
Estas operações têm o potencial de serem utilizadas bastantes vezes, para definir estas operações podemos usar *macros*:
Exemplo em C:
- #define GET_AZUL(cor) (cor & 0x001F)
- #define SET_BLUE(color, val)\(color, (val & 0x1F))

### Structs:
Podemos encaixar todas estas ferramentas num só item:
`struct rgb{
  uint16_t red: 5;
  uint16_t green: 6;
  uint16_t blue: 5;
};`
E depois o compilador trabalha por nós, faz as máscaras e tudo, por isso podemos fazer coisas desse tipo:
``` rgb cor;
cor.blue = 3; ```

#### NOTA:
Big-Endian VS Little-Endian:
left to right memory alloc vs right to left memory alloc.

#### Agora aplicando ao Timer:
struct control {
  //encher a partir do material de referência.
} -> não esquecer que intel e arm definem-se ao contrário. mas aquilo é corrido em virtual box em LCOM, por isso a máquina é igual para todos.

Quem perceber isto, é contratado na Synopsys ou Intel. Low-level pensa em poupar memória, high-level já são gulosos. (Prof. Nuno certified (ele recrutava pessoas))

### Apontadores:
Antes de cristo e depois de cristo. Apontadores são a nova vinda, os mais fortes aguentam. Não há garbage collector como no java, não se deixa ficar *lixo*. C bem feito, entra na cantina, senta-se num lugar, come e limpa. C mal feito limpa o prato de quem está sem querer a comer, ou vais buscar pratos que não são dele, coisas do género. Faltam no mundo programadores que limpem os pratos.
Inteiro ocupa o espaço mínimo da máquina, *tipo nativo*, é elementar.
Valor do apontador é onde começa, tipo define onde vai acabar. Apontador também está numa zona de memória, mas aponta pra outra.
Um apontador ocupa sempre o *tipo nativo*, como o inteiro. O seu endereço não muda porque é atribuído pelo OS.
Quando se incrementa o apontador, incrementa-se pelo tamanho do valor para o qual aponta.

` *p = null -> inválido `

Temos de ver casos a caso, cumprir os testes, certificarmo-nos do caso null, e indicar o erro.

### Enumerados:
É como um Word, como o nome diz, enumerar coisas fazer uma lista que começa no 0.


### Extend:
Uma variável aqui usada, está definida noutro ficheiro.

### Union:
Dá muito jeito.

Exemplo:
Color
A R G B (8 bits cada, 32 total)
(A é tranparência)
p = 0x12345678
Várias perspectivas:
Posso querer ver a cor toda, só o azul, etc. Quase como polimorfismo.

``` struct field{
  uint8_t a; // 1 byte
  uint16_t b; // 2
  uint32_t c; // 4
}; ``` poderia haver padding, vamos considerar que não, mas nesse caso invertia-se a ordem de definição, são 7 bytes.

podemos guardar todos este valores só ocupando o tamanho do maior. sobreposição. dá jeito para eficiência de memória. e efetivamente permite várias definições.
exemplos:
```union color{
  uint32_t i32;
  struct field{
    uint8_t A;
    uint8_t R;
    uint8_t G;
    uint8_t B;
  }
}; ``` cor é tudo, separada em 4 valores definidos diferentes.

```union float32{
  uint32_t value;
  struct field{
    uint32_t sinal : 1;
    uint32_t exp : 8;
    uint32_t mantissa : 23; // soma deles dá tamanho do tipo, 32.
  }
}; ```

Tudo isto permite-me gerar dados, alterar e armazenar de forma comfortável. 