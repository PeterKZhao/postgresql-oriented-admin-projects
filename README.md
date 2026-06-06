# PostgreSQL-Oriented Admin And Low-Code Projects

This repository collects a short, practical comparison of open-source projects adjacent to `gin-vue-admin`: admin platforms, low-code/internal-tool builders, CMS systems, and automatic API platforms that naturally lean toward PostgreSQL.

## Conclusion

For `gin-vue-admin` itself, MySQL is still the lower-friction choice because its default configuration and ecosystem path are MySQL-first. But if the goal is to study projects where PostgreSQL is a natural center of gravity, the best references are Directus, Baserow, ToolJet, Hasura, Supabase, Teable, Payload CMS, and Strapi.

## Recommended Projects

| Rank | Project | GitHub | Main Language | Positioning | PostgreSQL Fit |
|---:|---|---|---|---|---|
| 1 | Directus | `directus/directus` | TypeScript | Turns a database into an admin app, headless CMS, APIs, auth, and permissions | Strong fit. Very useful if you like database-first systems. Official Docker setups commonly use Postgres/PostGIS. |
| 2 | Baserow | `baserow/baserow` | Python / TypeScript | Open-source Airtable-like low-code data platform | Strong fit. PostgreSQL is central to its storage model. |
| 3 | ToolJet | `ToolJet/ToolJet` | JavaScript / TypeScript | Open-source Retool-style internal tool builder | Strong fit. Docker Compose includes Postgres and PostgREST. Good for internal tools and dashboards. |
| 4 | Hasura GraphQL Engine | `hasura/graphql-engine` | Haskell | Auto-generates GraphQL APIs, permissions, and subscriptions from PostgreSQL | Very strong fit. PostgreSQL-first by design. |
| 5 | Supabase | `supabase/supabase` | TypeScript | Postgres application platform with Auth, Storage, Realtime, Edge Functions | Very strong fit. The whole platform is built around PostgreSQL. |
| 6 | Teable | `teableio/teable` | TypeScript | No-code Postgres / Airtable alternative | Strong fit. The product positioning itself is Postgres-centered. |
| 7 | Payload CMS | `payloadcms/payload` | TypeScript | Headless CMS and full-stack admin framework | Good fit. Supports PostgreSQL and provides official Postgres templates. |
| 8 | Strapi | `strapi/strapi` | TypeScript | Headless CMS and admin panel | Moderate fit. Commonly used with PostgreSQL in production, but not strictly PostgreSQL-first. |

## How To Read The List

If you want alternatives or reference ideas close to `gin-vue-admin`, start with:

1. Directus
2. ToolJet
3. Baserow
4. Payload CMS

If you want PostgreSQL-first platform architecture, start with:

1. Supabase
2. Hasura
3. Teable
4. Baserow

If you only want projects that naturally choose PostgreSQL for production, focus on:

1. Directus
2. Baserow
3. ToolJet
4. Hasura
5. Supabase

## Notes

Appsmith and NocoDB are also well-known in the same general space, but they are better described as projects that can connect to PostgreSQL rather than projects whose own architecture is clearly PostgreSQL-first. They are useful references, but not first-choice examples for this specific question.

## Related Decision For Gin Vue Admin

For `gin-vue-admin`, the practical deployment recommendation remains:

- Use MySQL 8.x as the primary database.
- Keep Redis available for runtime cache/session/queue-style needs.
- Do not choose MongoDB just because the project has AI-assisted features.
- Consider PostgreSQL only if the surrounding product direction already benefits from PostgreSQL-specific capabilities.
