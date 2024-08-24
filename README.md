
# 🌐Desafio Fábrica de Software (Back-End) - UNIPÊ 2024.2🌐

# Minhas considerações 🌠

  Trabalhar nesse projeto foi uma experiencia e tanto, ter que fazer um desafio em menos de dois dias foi super puxado e desafiador, além disso ver como fuciona uma API foi muito legal e divertido, até então o meu primeiro contato com API e Djungo foi nesse semetre na fabrica e tem sido divertido trabalhar com isso.


## OBJETIVO DO DESAFIO :
  Criar um Projeto Django em Template ou API.

  O desafio deve possui Crud do Django com duas ou mais entidades e possuir a capacidade de consumir uma API externa gratuita(Sem token de autenticação) da escolha do usuário para guardar um dado.


## Requerimentos
1. asgiref
2. certifi
3. charset-normalizer
4. Django
5. djangorestframework
6. requests
7. mais sobre no próprios arquivo requirements.txt...

# instalações necessarias 
É necessário fazer algumas ações. Certifique-se de ter o Python e instalados. Em seguida, crie um novo ambiente virtual ```py -m venv {nome_da_venv}```, e __entre nela__ ```.\{nome_da_venv}\Scripts\activate```.
<br><br>
Em seguinda instale as __dependências:__
```
pip install djangorestframework
pip install requests
```
# API 🌐

### Escolha
A princípio, minha prioridade era escolher uma API simples, que me retornasse dados de maneira limpa e que de certa forma fossem fáceis de manipular. A API em questão foi a [Open Library Books API]([https://restcountries.com/](https://rapidapi.com/blog/directory/open-library-books/)), que fornece dados sobre livros com base em ISBNs e outros identificadores, oferecendo informações como título, autor e detalhes de publicação.
### intalação da API passo a passo

Antes de qualquer coisa é necessario dentro do seu aplicativo Django:
Configurar o seu Arquivo views.py dessa maneira:
```
import requests
from django.shortcuts import render, redirect
from django.contrib import messages

def buscar_livro_por_isbn(isbn):
    url = f'https://openlibrary.org/api/books?bibkeys=ISBN:{isbn}&format=json&jscmd=data'
    response = requests.get(url)
    return response.json()

def adicionar_livro(request):
    if request.method == 'POST':
        isbn = request.POST.get('isbn')
        data = buscar_livro_por_isbn(isbn)
        book_key = f'ISBN:{isbn}'
        if book_key in data:
            book = data[book_key]
            # Processar os dados do livro e salvar no banco de dados
            # Exemplo: Livro.objects.create(titulo=book['title'], ...)
            messages.success(request, f"Livro {book['title']} adicionado com sucesso!")
            return redirect('livro_list')
        else:
            messages.error(request, "Livro não encontrado na API.")
    return render(request, 'adicionar_livro.html')
 ```
E depois na urls.py tambem configurar desse jeito:
```
from django.urls import path
from .views import adicionar_livro

urlpatterns = [
    path('adicionar/', adicionar_livro, name='adicionar_livro'),
]
 
```
E finalizando criando dentro do aplicativo uma pasta com o nome template e criar uma arquivo com o nome "adicionar_livro.html" e configurando desse jeito:
```
<!DOCTYPE html>
<html>
<head>
    <title>Adicionar Livro</title>
</head>
<body>
    <h1>Adicionar Novo Livro</h1>
    <form method="post">
        {% csrf_token %}
        <label for="isbn">ISBN:</label>
        <input type="text" id="isbn" name="isbn" required>
        <button type="submit">Adicionar Livro</button>
    </form>
    {% if messages %}
        <ul>
            {% for message in messages %}
                <li>{{ message }}</li>
            {% endfor %}
        </ul>
    {% endif %}
</body>
</html>

```














