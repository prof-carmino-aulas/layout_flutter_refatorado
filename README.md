

# Introdução do Widget Expanded e ListView no BodyConteudo.dart

Em Flutter, o Expanded é um widget de layout usado dentro de um row, column ou flex para fazer com que um filho ocupe o espaço disponível restante no exito principal, de outra forma o Expanded força o seu filho a exmpandir preencendo o espaço livre dentro de um layout  flexível 
```dart
// BodyConteudo.dart
// corpo do aplicativo
//
import 'package:flutter/material.dart';
import 'package:layout_flutter_refatorado/widgets/card_conteudo.dart';

class BodyConteudo extends StatelessWidget {
  const BodyConteudo({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.all(20),
      color: Colors.blue.shade100,
      child: Column(
        children: [
          const SizedBox(height: 20),
          const Text(
            "BODY\nÁrea Principal de Conteúdo",
            textAlign: TextAlign.center,
            style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 20),
          CardConteudo(),
        ],
      ),
    );
  }
}
```
Para entender melhor inclua
```diff
- CardConteudo(),
+ Expanded(
+   child: CardConteudo(),
+ ),
```

## Inserindo o ListView
```
É um widget de rolagem (scrollable) usado para exibir uma lista de elementos organizados sequencialmente — normalmente na vertical.
Quando você coloca um ListView dentro de uma Column, o Flutter gera erro se você não usar Expanded.
Porque: Column não define altura fixa para seus filhos e ListView precisa de altura definida para funcionar.
Sem Expanded, o Flutter lança erro de "Vertical viewport was given unbounded height".
```

```diff
- CardConteudo(),
+ Expanded(child: ListView(children: const [CardConteudo()])),
```

---
## Agora vamos colocar um selo sobre o card 

No **Flutter**, `Stack` é um widget de layout que permite **sobrepor (empilhar) widgets uns sobre os outros**.
Diferente de `Row` e `Column`, que organizam elementos **sequencialmente**, o `Stack` organiza elementos em **camadas (layers)** no mesmo espaço.

---
## Conceito
> `Stack` permite posicionamento relativo e sobreposição de widgets no mesmo eixo bidimensional.
Ele funciona como um sistema de camadas:

```
Camada 3 (topo)
Camada 2
Camada 1 (base)
```
O último widget da lista fica **por cima** dos anteriores.

## Estrutura básica

```dart
Stack(
  children: [
    Container(color: Colors.blue),
    Text("Sobreposto"),
  ],
)
```
O `Container`é a base, e o `Text` ficará por cima 

## Uso com `Positioned`
O `Stack` normalmente trabalha junto com `Positioned`.

```dart
Stack(
  children: [
    Container(
      width: 200,
      height: 200,
      color: Colors.blue,
    ),
    Positioned(
      top: 10,
      right: 10,
      child: Icon(Icons.star, color: Colors.white),
    ),
  ],
)
```
* O ícone fica no canto superior direito, sendo que o posicionamento é absoluto dentro do `Stack`.

## Como o layout funciona internamente

1. O `Stack` recebe as constraints do pai.
2. Define seu próprio tamanho.
3. Posiciona os filhos:
   * Sem `Positioned` → alinhamento padrão (canto superior esquerdo).
   * Com `Positioned` → usa coordenadas absolutas.

## Propriedade importante: `alignment`

```dart
Stack(
  alignment: Alignment.center,
  children: [
    Container(width: 200, height: 200, color: Colors.blue),
    Text("Centro"),
  ],
)
```
Aqui, os filhos não posicionados ficam centralizados.

## Limitações importantes

`Stack` não faz rolagem, não organiza elementos em sequência, não divide espaço automaticamente
Ele apenas sobrepõe.

## Diferença entre Stack e Column/Row

| Widget   | Organiza   | Sobrepõe | Scroll |
| -------- | ---------- | -------- | ------ |
| Column   | Vertical   | ❌        | ❌      |
| Row      | Horizontal | ❌        | ❌      |
| Stack    | Camadas    | ✅        | ❌      |
| ListView | Sequencial | ❌        | ✅      |

## Quando usar Stack
Badge sobre ícone, Selo “NOVO” em card, Botão flutuante sobre imagem, Texto sobre imagem, Layout tipo overlay

Para ver o funcionamento crie o arquivo selo.dart na pasta widgets 

```dart
import 'package:flutter/material.dart';

class Selo extends StatelessWidget {
  final String texto;
  final Color cor;

  const Selo({super.key, required this.texto, this.cor = Colors.red});

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
      decoration: BoxDecoration(
        color: cor,
        borderRadius: BorderRadius.circular(12),
      ),
      child: Text(
        texto,
        style: const TextStyle(
          color: Colors.white,
          fontSize: 12,
          fontWeight: FontWeight.bold,
        ),
      ),
    );
  }
}
```
Agora vamos alterar o card_conteudo para receber dois parametros e usar o Stack 

```dart
import 'package:flutter/material.dart';
import 'package:layout_flutter_refatorado/widgets/item_lista.dart';
import 'package:layout_flutter_refatorado/widgets/selo.dart';

class CardConteudo extends StatelessWidget {
  final bool mostrarSelo;
  final String textoSelo;

  const CardConteudo({
    super.key,
    this.mostrarSelo = false,
    this.textoSelo = "NOVO",
  });

  @override
  Widget build(BuildContext context) {
    final card = Card(
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(20),
        side: const BorderSide(color: Colors.indigo, width: 2),
      ),
      elevation: 6,
      child: const Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              "Aqui ficam os textos",
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 10),
            ItemLista(icon: Icons.text_fields, label: "Textos"),
            ItemLista(icon: Icons.list, label: "Lista"),
            ItemLista(icon: Icons.edit, label: "Formulário"),
            ItemLista(icon: Icons.dashboard, label: "Gráficos e layouts"),
          ],
        ),
      ),
    );

    if (!mostrarSelo) return card;

    return Stack(
      clipBehavior: Clip.none,
      children: [
        card,
        Positioned(top: 8, right: 8, child: Selo(texto: textoSelo)),
      ],
    );
  }
}
```
