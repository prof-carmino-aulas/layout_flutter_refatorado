

# Introdução do Widget Expanded no BodyConteudo.dart

---
Em Flutter, o Expanded é um widget de layout usado dentro de um row, column ou flex para fazer com que um filho ocupe o espaço disponível restante no exito principal, de outra forma o Expanded força o seu filho a exmpandir preencendo o espaço livre dentro de um layout  flexível 

---

## Código Fonte

```dart
//corpo do aplicativo
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

Para entender melhor incluimos o trecho 


```dart
//corpo do aplicativo
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
          <span style="color:green">
          CardConteudo(),
```diff
// ALTERAÇÃO: inclusão de Expanded para controle de layout
- CardConteudo(),
+ Expanded(
+   child: CardConteudo(),
+ ),
          </span>
        ],
      ),
    );
  }
}
```

