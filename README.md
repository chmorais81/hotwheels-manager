# Sistema de Gerenciamento de Coleções Hot Wheels

Este documento detalha a arquitetura, tecnologias e esquema do banco de dados para o Sistema de Gerenciamento de Coleções Hot Wheels.

## 1. Visão Geral

O sistema proposto é uma aplicação multiusuário para colecionadores de Hot Wheels, permitindo o gerenciamento de suas coleções, visualização de estatísticas e interação através de uma API RESTful. O foco é em uma experiência rica e gamificada para o usuário.

## 2. Tecnologias Utilizadas

### Backend
- **Linguagem:** PHP 8.1+
- **Framework/Arquitetura:** MVC (Model-View-Controller)
- **Acesso a Dados:** PDO (PHP Data Objects)
- **Padrões:** PSR-4 (Autoloading Standard)
- **Segurança:** `password_hash`, CSRF tokens, Prepared Statements, Session Hardening
- **Testes:** PHPUnit com mocks e SQLite in-memory

### Frontend
- **Biblioteca:** React
- **Estilização:** Tailwind CSS
- **Componentes UI:** shadcn/ui
- **Animações:** Framer Motion
- **Gráficos:** Recharts (para dashboard)

### Banco de Dados
- **Provedor:** Supabase / MySQL
- **Ferramenta de Orquestração:** Docker (para app + db)

### API
- **Formato:** RESTful (JSON)
- **Autenticação:** JWT (JSON Web Tokens) para mobile
- **Versionamento:** `/api/v1`

## 3. Estrutura do Projeto (Esboço)

```
hotwheels-collection-manager/
├── backend/
│   ├── app/
│   │   ├── Controllers/
│   │   ├── Models/
│   │   ├── Views/
│   │   ├── Core/ (Classes base, Router, etc.)
│   │   └── Middleware/
│   ├── config/
│   ├── public/ (Front Controller)
│   ├── routes/
│   ├── tests/
│   ├── vendor/
│   └── composer.json
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── contexts/
│   │   ├── hooks/
│   │   └── App.js
│   ├── tailwind.config.js
│   ├── package.json
│   └── postcss.config.js
├── docker-compose.yml
├── .env.example
└── README.md
```

## 4. Esquema do Banco de Dados

### Tabela `users`
- `id` (INT, PK, AUTO_INCREMENT)
- `username` (VARCHAR(255), UNIQUE, NOT NULL)
- `email` (VARCHAR(255), UNIQUE, NOT NULL)
- `password_hash` (VARCHAR(255), NOT NULL)
- `role` (ENUM('Admin', 'User'), DEFAULT 'User', NOT NULL)
- `avatar_url` (VARCHAR(255), NULL)
- `created_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- `updated_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP)

### Tabela `collections`
- `id` (INT, PK, AUTO_INCREMENT)
- `user_id` (INT, FK para `users.id`, NOT NULL)
- `name` (VARCHAR(255), NOT NULL)
- `description` (TEXT, NULL)
- `created_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- `updated_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP)

### Tabela `cars`
- `id` (INT, PK, AUTO_INCREMENT)
- `collection_id` (INT, FK para `collections.id`, NOT NULL)
- `name` (VARCHAR(255), NOT NULL)
- `brand` (VARCHAR(255), NOT NULL)
- `year` (INT, NOT NULL)
- `miniature_number` (VARCHAR(50), NULL)
- `series` (VARCHAR(255), NULL)
- `condition` (ENUM('Mint', 'Excellent', 'Good', 'Fair', 'Poor'), NOT NULL)
- `price_paid` (DECIMAL(10, 2), NULL)
- `photo_url` (VARCHAR(255), NULL)
- `thumbnail_url` (VARCHAR(255), NULL)
- `created_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- `updated_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP)

### Tabela `achievements`
- `id` (INT, PK, AUTO_INCREMENT)
- `user_id` (INT, FK para `users.id`, NOT NULL)
- `name` (VARCHAR(255), NOT NULL)
- `description` (TEXT, NULL)
- `icon_url` (VARCHAR(255), NULL)
- `achieved_at` (DATETIME, DEFAULT CURRENT_TIMESTAMP)

## 5. Considerações de Segurança

- **Autenticação:** Uso de `password_hash` para senhas, JWT para API mobile.
- **Autorização:** Middleware para controle de acesso baseado em perfis (Admin/User).
- **CSRF:** Proteção contra Cross-Site Request Forgery em formulários.
- **SQL Injection:** Uso de Prepared Statements com PDO.
- **Upload de Arquivos:** Validação rigorosa de tipo, tamanho e conteúdo de imagens.
- **Sessões:** Configurações seguras de sessão (HttpOnly, Secure, SameSite).

## 6. Próximos Passos

1. Configurar o ambiente de desenvolvimento com Docker.
2. Implementar o esquema do banco de dados no MySQL/Supabase.
3. Desenvolver o backend PHP para autenticação e gestão de usuários.
4. Iniciar o desenvolvimento do frontend React.
