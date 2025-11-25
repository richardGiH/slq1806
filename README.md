//Prof. Alexandre Garcez Vieira
//
//
import 'package:flutter/material.dart';
import 'package:flutter/services.dart'; // Para "TextInputFormatter"

void main() {
  runApp(MyApp());
}

// Classe do Modelo de Dados (Adaptada)
class Coleta {
  String id;
  String tipoResiduo; // Era 'name'
  String classificacao; // Era 'category'
  String observacoes; // Era 'description'
  double pesoKg; // Era 'price'
  String imageUrl;

  Coleta({
    required this.id,
    required this.tipoResiduo,
    required this.classificacao,
    required this.observacoes,
    required this.pesoKg,
    required this.imageUrl,
  });
}

// Ponto de entrada da Aplicação
class MyApp extends StatelessWidget {
  // Cor principal baseada nos protótipos
  static const MaterialColor mainAppColor = MaterialColor(
    0xFF4A148C, // Um roxo profundo, próximo ao dos protótipos
    <int, Color>{
      50: Color(0xFFE0B0FF),
      100: Color(0xFFD394FF),
      200: Color(0xFFC57AFF),
      300: Color(0xFFB860FF),
      400: Color(0xFFA142F8),
      500: Color(0xFF8A2BE2), // Violeta/Roxo
      600: Color(0xFF7B24CB),
      700: Color(0xFF6A1FB8),
      800: Color(0xFF5A1AA4),
      900: Color(0xFF4A148C), // Roxo escuro
    },
  );

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Meu App Coleta',
      theme: ThemeData(
        primarySwatch: mainAppColor,
        visualDensity: VisualDensity.adaptivePlatformDensity,
        scaffoldBackgroundColor: Color(0xFFF5F5F5), // Um fundo cinza claro
        appBarTheme: AppBarTheme(
          backgroundColor: mainAppColor,
          foregroundColor: Colors.white, // Cor do texto e ícones na AppBar
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: mainAppColor,
            foregroundColor: Colors.white,
            padding: EdgeInsets.symmetric(vertical: 16.0),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8.0),
            ),
          ),
        ),
        floatingActionButtonTheme: FloatingActionButtonThemeData(
          backgroundColor: mainAppColor,
          foregroundColor: Colors.white,
        ),
        cardTheme: CardTheme(
          elevation: 4.0,
          margin: EdgeInsets.symmetric(horizontal: 10.0, vertical: 6.0),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12.0),
          ),
        ),
      ),
      debugShowCheckedModeBanner: false,
      // Define as rotas
      initialRoute: '/login',
      routes: {
        '/login': (context) => LoginScreen(),
        '/error': (context) => ErrorScreen(),
        '/list': (context) => ColetaListScreen(), // Rota para a lista de coletas
      },
    );
  }
}

// --- Tela de Login (1) --- (Sem alterações)
class LoginScreen extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final _userController = TextEditingController(text: 'admin@exemplo.com');
  final _passController = TextEditingController(text: '123456');
  bool _obscureText = true;

  void _login() {
    // Credenciais fixas para o exemplo
    if (_userController.text == 'admin@exemplo.com' &&
        _passController.text == '123456') {
      // Sucesso: Substitui a tela de login pela lista
      Navigator.pushReplacementNamed(context, '/list');
    } else {
      // Erro: Mostra a tela de erro
      Navigator.pushNamed(context, '/error');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Meu App - Login'),
        automaticallyImplyLeading: false, // Remove a seta de "voltar"
      ),
      body: Center(
        child: SingleChildScrollView(
          padding: EdgeInsets.all(24.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Text(
                'Entre com Suas Credenciais',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
              ),
              SizedBox(height: 40),
              TextField(
                controller: _userController,
                decoration: InputDecoration(
                  labelText: 'Usuário',
                  prefixIcon: Icon(Icons.person),
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(8.0),
                  ),
                ),
                keyboardType: TextInputType.emailAddress,
              ),
              SizedBox(height: 20),
              TextField(
                controller: _passController,
                obscureText: _obscureText,
                decoration: InputDecoration(
                  labelText: 'Senha',
                  prefixIcon: Icon(Icons.lock),
                  suffixIcon: IconButton(
                    icon: Icon(
                      _obscureText ? Icons.visibility_off : Icons.visibility,
                    ),
                    onPressed: () {
                      setState(() {
                        _obscureText = !_obscureText;
                      });
                    },
                  ),
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(8.0),
                  ),
                ),
              ),
              SizedBox(height: 40),
              ElevatedButton(
                child: Text('Entrar', style: TextStyle(fontSize: 18)),
                onPressed: _login,
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// --- Tela de Erro (2) --- (Sem alterações)
class ErrorScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Meu App - Erro'),
        automaticallyImplyLeading: false,
      ),
      body: Center(
        child: Padding(
          padding: EdgeInsets.all(24.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Icon(
                Icons.error_outline,
                color: Colors.red,
                size: 100,
              ),
              SizedBox(height: 20),
              Text(
                'Credenciais Inválidas',
                textAlign: TextAlign.center,
                style: TextStyle(
                  fontSize: 26,
                  fontWeight: FontWeight.bold,
                  color: Colors.red,
                ),
              ),
              SizedBox(height: 10),
              Text(
                'Tente novamente',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 16, color: Colors.grey[700]),
              ),
              SizedBox(height: 40),
              ElevatedButton(
                child: Text('Voltar para Login', style: TextStyle(fontSize: 18)),
                onPressed: () {
                  // Volta para a tela anterior (Login)
                  Navigator.pop(context);
                },
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// --- Tela de Lista de Coletas (3) --- (Adaptada)
class ColetaListScreen extends StatefulWidget {
  @override
  _ColetaListScreenState createState() => _ColetaListScreenState();
}

class _ColetaListScreenState extends State<ColetaListScreen> {
  // Dados mocados (mock data) iniciais, conforme protótipo adaptado
  final List<Coleta> _coletas = [
    Coleta(
      id: '1',
      tipoResiduo: 'Plástico (Garrafas PET)',
      classificacao: 'Reciclável',
      observacoes: 'Sacos grandes, limpos e secos',
      pesoKg: 2.5,
      imageUrl: 'https://placehold.co/100x100/E8E8E8/000000?text=Plastico',
    ),
    Coleta(
      id: '2',
      tipoResiduo: 'Papel e Papelão',
      classificacao: 'Reciclável',
      observacoes: 'Caixas desmontadas e jornais',
      pesoKg: 5.0,
      imageUrl: 'https://placehold.co/100x100/E8E8E8/000000?text=Papel',
    ),
    Coleta(
      id: '3',
      tipoResiduo: 'Resíduos Orgânicos',
      classificacao: 'Orgânico',
      observacoes: 'Restos de alimentos, cascas de frutas',
      pesoKg: 1.5,
      imageUrl: 'https://placehold.co/100x100/E8E8E8/000000?text=Organico',
    ),
    Coleta(
      id: '4',
      tipoResiduo: 'Vidro (Potes e Garrafas)',
      classificacao: 'Reciclável',
      observacoes: 'Embalagens de vidro, potes',
      pesoKg: 3.0,
      imageUrl: 'https://placehold.co/100x100/E8E8E8/000000?text=Vidro',
    ),
  ];

  // Calcula o total de peso
  double _calculateTotalPeso() {
    return _coletas.fold(0.0, (sum, item) => sum + item.pesoKg);
  }

  // Navega para a tela de adicionar
  void _navigateToAddColeta() async {
    final newColeta = await Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => AddColetaScreen()),
    );

    if (newColeta != null && newColeta is Coleta) {
      setState(() {
        _coletas.add(newColeta);
      });
    }
  }

  // Navega para a tela de editar
  void _navigateToEditColeta(Coleta coleta, int index) async {
    final updatedColeta = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => EditColetaScreen(coleta: coleta),
      ),
    );

    if (updatedColeta != null && updatedColeta is Coleta) {
      setState(() {
        _coletas[index] = updatedColeta;
      });
    }
  }

  // Exclui um registro de coleta
  void _deleteColeta(String id) {
    setState(() {
      _coletas.removeWhere((coleta) => coleta.id == id);
    });
  }

  // Formata o peso para "X,X Kg"
  String _formatPeso(double peso) {
    return '${peso.toStringAsFixed(2).replaceAll('.', ',')} Kg';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Meu App - Lista de Coletas'),
      ),
      body: Column(
        children: [
          Expanded(
            child: ListView.builder(
              itemCount: _coletas.length,
              itemBuilder: (context, index) {
                final coleta = _coletas[index];
                return ColetaCard(
                  coleta: coleta,
                  onEdit: () => _navigateToEditColeta(coleta, index),
                  onDelete: () => _deleteColeta(coleta.id),
                  formatPeso: _formatPeso,
                );
              },
            ),
          ),
          // Rodapé com o Total
          Container(
            padding: EdgeInsets.symmetric(horizontal: 20, vertical: 16),
            decoration: BoxDecoration(
              color: Theme.of(context).primaryColor,
              borderRadius: BorderRadius.only(
                topLeft: Radius.circular(20),
                topRight: Radius.circular(20),
              ),
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.1),
                  blurRadius: 10,
                  offset: Offset(0, -5),
                ),
              ],
            ),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  'Total Coletado:',
                  style: TextStyle(
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                  ),
                ),
                Text(
                  _formatPeso(_calculateTotalPeso()),
                  style: TextStyle(
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _navigateToAddColeta,
        child: Icon(Icons.add),
        tooltip: 'Registrar Coleta',
      ),
    );
  }
}

// Card customizado para a Coleta (Adaptado)
class ColetaCard extends StatelessWidget {
  final Coleta coleta;
  final VoidCallback onEdit;
  final VoidCallback onDelete;
  final String Function(double) formatPeso;

  const ColetaCard({
    Key? key,
    required this.coleta,
    required this.onEdit,
    required this.onDelete,
    required this.formatPeso,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Card(
      child: InkWell(
        onTap: onEdit, // Permite editar ao tocar no card
        child: Padding(
          padding: EdgeInsets.all(12.0),
          child: Row(
            children: [
              ClipRRect(
                borderRadius: BorderRadius.circular(8.0),
                child: Image.network(
                  coleta.imageUrl,
                  width: 80,
                  height: 80,
                  fit: BoxFit.cover,
                  errorBuilder: (context, error, stackTrace) => Container(
                    width: 80,
                    height: 80,
                    color: Colors.grey[200],
                    child: Icon(Icons.recycling, color: Colors.grey[400]),
                  ),
                ),
              ),
              SizedBox(width: 16.0),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      coleta.tipoResiduo,
                      style:
                          TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                      overflow: TextOverflow.ellipsis,
                    ),
                    SizedBox(height: 4.0),
                    Text(
                      coleta.classificacao,
                      style: TextStyle(fontSize: 14, color: Colors.grey[600]),
                    ),
                    SizedBox(height: 4.0),
                    Text(
                      coleta.observacoes,
                      style: TextStyle(fontSize: 14),
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
                    ),
                    SizedBox(height: 8.0),
                    Text(
                      formatPeso(coleta.pesoKg),
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.bold,
                        color: Theme.of(context).primaryColor,
                      ),
                    ),
                  ],
                ),
              ),
              SizedBox(width: 8.0),
              IconButton(
                icon: Icon(Icons.delete_outline, color: Colors.red[700]),
                onPressed: onDelete,
                tooltip: 'Excluir',
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// --- Tela de Adicionar Coleta (4) --- (Adaptada)
class AddColetaScreen extends StatefulWidget {
  @override
  _AddColetaScreenState createState() => _AddColetaScreenState();
}

class _AddColetaScreenState extends State<AddColetaScreen> {
  final _formKey = GlobalKey<FormState>();
  final _tipoResiduoController = TextEditingController();
  final _obsController = TextEditingController();
  final _pesoController = TextEditingController();
  final _imageUrlController = TextEditingController(); // Mantido

  String? _selectedClassificacao;
  final List<String> _classificacoes = [
    'Reciclável',
    'Orgânico',
    'Rejeito',
    'Outros'
  ];

  void _saveColeta() {
    // Valida o formulário
    if (_formKey.currentState!.validate()) {
      // Cria a nova coleta
      final newColeta = Coleta(
        id: DateTime.now().millisecondsSinceEpoch.toString(),
        tipoResiduo: _tipoResiduoController.text,
        classificacao: _selectedClassificacao!,
        observacoes: _obsController.text,
        pesoKg: double.tryParse(_pesoController.text.replaceAll(',', '.')) ?? 0.0,
        imageUrl: _imageUrlController.text.isNotEmpty
            ? _imageUrlController.text
            : 'https://placehold.co/100x100/E8E8E8/000000?text=Novo',
      );

      // Retorna a nova coleta para a tela anterior
      Navigator.pop(context, newColeta);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Meu App - Registrar Coleta'),
      ),
      body: Form(
        key: _formKey,
        child: ListView(
          padding: EdgeInsets.all(16.0),
          children: [
            // Tipo de Resíduo
            TextFormField(
              controller: _tipoResiduoController,
              decoration: InputDecoration(
                labelText: 'Tipo de Resíduo *',
                hintText: 'Ex: Garrafas PET e Embalagens',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              maxLength: 120,
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe o Tipo de Resíduo';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Classificação
            DropdownButtonFormField<String>(
              value: _selectedClassificacao,
              decoration: InputDecoration(
                labelText: 'Classificação *',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              hint: Text('Selecione a Classificação'),
              items: _classificacoes.map((String classificacao) {
                return DropdownMenuItem<String>(
                  value: classificacao,
                  child: Text(classificacao),
                );
              }).toList(),
              onChanged: (newValue) {
                setState(() {
                  _selectedClassificacao = newValue;
                });
              },
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Selecione a Classificação';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Observações
            TextFormField(
              controller: _obsController,
              decoration: InputDecoration(
                labelText: 'Observações *',
                hintText: 'Descreva o resíduo e o volume',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              maxLength: 500,
              maxLines: 4,
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe as Observações';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Peso
            TextFormField(
              controller: _pesoController,
              decoration: InputDecoration(
                labelText: 'Peso (Kg) *',
                hintText: '0,00',
                suffixText: 'Kg',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              keyboardType: TextInputType.numberWithOptions(decimal: true),
              inputFormatters: [
                FilteringTextInputFormatter.allow(RegExp(r'^\d+[,.]?\d{0,2}')),
              ],
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe o Peso';
                }
                if (double.tryParse(value.replaceAll(',', '.')) == null) {
                  return 'Peso inválido';
                }
                return null;
              },
            ),
            SizedBox(height: 16),
            
            // URL da Imagem (Opcional)
            TextFormField(
              controller: _imageUrlController,
              decoration: InputDecoration(
                labelText: 'URL da Imagem (Opcional)',
                hintText: 'https://...',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              keyboardType: TextInputType.url,
            ),
            SizedBox(height: 32),

            // Botões
            Row(
              children: [
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {
                      Navigator.pop(context); // Apenas fecha a tela
                    },
                    child: Text('Cancelar'),
                    style: OutlinedButton.styleFrom(
                      padding: EdgeInsets.symmetric(vertical: 16.0),
                      foregroundColor: Theme.of(context).primaryColor,
                      side: BorderSide(color: Theme.of(context).primaryColor),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(8.0),
                      ),
                    ),
                  ),
                ),
                SizedBox(width: 16),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _saveColeta,
                    child: Text('Salvar'),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

// --- Tela de Editar Coleta (5) --- (Adaptada)
class EditColetaScreen extends StatefulWidget {
  final Coleta coleta;

  const EditColetaScreen({Key? key, required this.coleta}) : super(key: key);

  @override
  _EditColetaScreenState createState() => _EditColetaScreenState();
}

class _EditColetaScreenState extends State<EditColetaScreen> {
  final _formKey = GlobalKey<FormState>();
  late TextEditingController _tipoResiduoController;
  late TextEditingController _obsController;
  late TextEditingController _pesoController;
  late TextEditingController _imageUrlController;
  
  String? _selectedClassificacao;
  final List<String> _classificacoes = [
    'Reciclável',
    'Orgânico',
    'Rejeito',
    'Outros'
  ];

  @override
  void initState() {
    super.initState();
    // Inicializa os controladores com os dados da coleta
    _tipoResiduoController = TextEditingController(text: widget.coleta.tipoResiduo);
    _obsController = TextEditingController(text: widget.coleta.observacoes);
    _pesoController = TextEditingController(
        text: widget.coleta.pesoKg.toStringAsFixed(2).replaceAll('.', ','));
    _imageUrlController = TextEditingController(text: widget.coleta.imageUrl);
    _selectedClassificacao = widget.coleta.classificacao;
  }

  void _saveChanges() {
    if (_formKey.currentState!.validate()) {
      // Cria a coleta atualizada
      final updatedColeta = Coleta(
        id: widget.coleta.id, // Mantém o ID original
        tipoResiduo: _tipoResiduoController.text,
        classificacao: _selectedClassificacao!,
        observacoes: _obsController.text,
        pesoKg: double.tryParse(_pesoController.text.replaceAll(',', '.')) ?? 0.0,
        imageUrl: _imageUrlController.text.isNotEmpty
            ? _imageUrlController.text
            : 'https://placehold.co/100x100/E8E8E8/000000?text=Editado',
      );

      // Retorna a coleta atualizada
      Navigator.pop(context, updatedColeta);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Editar Coleta'),
      ),
      body: Form(
        key: _formKey,
        child: ListView(
          padding: EdgeInsets.all(16.0),
          children: [
            // Tipo de Resíduo
            TextFormField(
              controller: _tipoResiduoController,
              decoration: InputDecoration(
                labelText: 'Tipo de Resíduo *',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              maxLength: 120,
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe o Tipo de Resíduo';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Classificação
            DropdownButtonFormField<String>(
              value: _selectedClassificacao,
              decoration: InputDecoration(
                labelText: 'Classificação *',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              items: _classificacoes.map((String classificacao) {
                return DropdownMenuItem<String>(
                  value: classificacao,
                  child: Text(classificacao),
                );
              }).toList(),
              onChanged: (newValue) {
                setState(() {
                  _selectedClassificacao = newValue;
                });
              },
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Selecione a Classificação';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Observações
            TextFormField(
              controller: _obsController,
              decoration: InputDecoration(
                labelText: 'Observações *',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              maxLength: 500,
              maxLines: 4,
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe as Observações';
                }
                return null;
              },
            ),
            SizedBox(height: 16),

            // Peso
            TextFormField(
              controller: _pesoController,
              decoration: InputDecoration(
                labelText: 'Peso (Kg) *',
                suffixText: 'Kg',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              keyboardType: TextInputType.numberWithOptions(decimal: true),
              inputFormatters: [
                FilteringTextInputFormatter.allow(RegExp(r'^\d+[,.]?\d{0,2}')),
              ],
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Informe o Peso';
                }
                if (double.tryParse(value.replaceAll(',', '.')) == null) {
                  return 'Peso inválido';
                }
                return null;
              },
            ),
            SizedBox(height: 16),
            
            // URL da Imagem
            TextFormField(
              controller: _imageUrlController,
              decoration: InputDecoration(
                labelText: 'URL da Imagem',
                hintText: 'https://...',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(8.0),
                ),
              ),
              keyboardType: TextInputType.url,
            ),
            SizedBox(height: 32),

            // Botões
            Row(
              children: [
                Expanded(
                  child: OutlinedButton(
                    onPressed: () {
                      Navigator.pop(context); // Apenas fecha a tela
                    },
                    child: Text('Cancelar'),
                    style: OutlinedButton.styleFrom(
                      padding: EdgeInsets.symmetric(vertical: 16.0),
                      foregroundColor: Theme.of(context).primaryColor,
                      side: BorderSide(color: Theme.of(context).primaryColor),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(8.0),
                      ),
                    ),
                  ),
                ),
                SizedBox(width: 16),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _saveChanges,
                    child: Text('Salvar Alterações'),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
