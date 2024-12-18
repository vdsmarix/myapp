import 'package:flutter/material.dart';
import 'package:uni_links/uni_links.dart';
import 'dart:async';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Deep Link Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  late StreamSubscription _sub;
  String _linkMessage = "Aguardando o link...";

  @override
  void initState() {
    super.initState();
    _initDeepLink();
  }

  // Inicializar o deep link
  Future<void> _initDeepLink() async {
    // Verifique o link no momento de iniciar
    try {
      final initialLink = await getInitialLink();
      if (initialLink != null) {
        _navigateToPage(initialLink);
      }
    } catch (e) {
      print("Erro ao tentar abrir o deep link: $e");
    }

    // Monitorar os links quando o app está aberto
    _sub = linkStream.listen((String? link) {
      if (link != null) {
        _navigateToPage(link);
      }
    });
  }

  // Função para navegar para a página correspondente ao deep link
  void _navigateToPage(String link) {
    // Navegar para a página conforme o link
    if (link == "meuapp://pagina2") {
      setState(() {
        _linkMessage = "Você foi redirecionado para a Página 2!";
      });
    }
  }

  @override
  void dispose() {
    _sub.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Página Inicial'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              _linkMessage,
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                // Simula o deep link ao clicar no botão
                // Isso poderia ser um link real do tipo meuapp://pagina2
                setState(() {
                  _linkMessage = "Você foi redirecionado para a Página 2!";
                });
              },
              child: Text('Simular Deep Link para Página 2'),
            ),
          ],
        ),
      ),
    );
  }
}


