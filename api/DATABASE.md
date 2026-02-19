
# User Service

```
# Users table: stores account credentials and profile info
CREATE TABLE users (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email       VARCHAR(255) UNIQUE NOT NULL,
  username    VARCHAR(100) UNIQUE NOT NULL,
  password    VARCHAR(255) NOT NULL,        -- bcrypt hashed
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```
# Notes table: stores note content and metadata

```
-- A denormalized copy of the user identity (synced via Kafka events)
CREATE TABLE users (
  id          UUID PRIMARY KEY,             -- same UUID from user-service
  username    VARCHAR(100) NOT NULL,        -- copied for display (e.g. author name)
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Notes table
CREATE TABLE notes (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title       VARCHAR(500) NOT NULL DEFAULT 'Untitled',
  content     TEXT,                         -- raw text or JSON (e.g. rich content blocks)
  is_archived BOOLEAN NOT NULL DEFAULT FALSE,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Optional: tags for notes
CREATE TABLE tags (
  id    UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name  VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE note_tags (
  note_id  UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
  tag_id   UUID NOT NULL REFERENCES tags(id)  ON DELETE CASCADE,
  PRIMARY KEY (note_id, tag_id)
);
```