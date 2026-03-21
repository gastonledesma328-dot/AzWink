# Weezyx — Setup

## Estructura del repo

```
/
├── index.html      ← app completa
├── netlify.toml    ← config de Netlify
└── README.md
```

---

## 1. Supabase — crear base de datos y storage

1. Entrá a https://supabase.com y creá una cuenta gratis
2. Creá un nuevo proyecto (guardá la contraseña)
3. En el panel, andá a **SQL Editor** y ejecutá:

```sql
create table submissions (
  id uuid default gen_random_uuid() primary key,
  username text not null,
  filename text not null,
  audio_url text not null,
  duration integer,
  created_at timestamptz default now()
);

alter table submissions enable row level security;

create policy "Lectura pública" on submissions
  for select using (true);

create policy "Inserción pública" on submissions
  for insert with check (true);
```

4. Andá a **Storage → New Bucket**
   - Nombre: `grabaciones`
   - Tildá **Public bucket**
   - Guardá

5. En **Storage → Policies** agregá política de insert:
   - Bucket: `grabaciones`
   - Allowed operation: `INSERT`
   - Policy: `true` (público)

6. Copiá tus credenciales desde **Settings → API**:
   - `Project URL` → reemplazá `TUPROYECTO.supabase.co` en index.html
   - `anon public key` → reemplazá `TU_ANON_KEY` en index.html

---

## 2. GitHub — subir el código

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/TUUSUARIO/weezyx.git
git push -u origin main
```

---

## 3. Netlify — conectar y deployar

1. Entrá a https://netlify.com
2. **Add new site → Import from Git**
3. Conectá tu cuenta de GitHub y elegí el repo `weezyx`
4. Dejá todo por defecto y hacé click en **Deploy site**
5. En ~30 segundos tu app está en vivo con HTTPS ✓

Netlify va a re-deployar automáticamente cada vez que hagas `git push`.

---

## Uso

- **Participantes** entran a la URL de Netlify, graban y envían
- **Admin** abre la misma URL, va al panel Admin y ve todas las grabaciones en tiempo real
