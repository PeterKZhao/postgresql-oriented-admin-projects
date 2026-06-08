# Admin And Low-Code Project Database Notes

This repository collects a short, practical comparison of open-source projects adjacent to `gin-vue-admin` and `RuoYi-Vue-Pro`: admin platforms, low-code/internal-tool builders, CMS systems, and automatic API platforms, grouped by whether they naturally lean toward PostgreSQL or MySQL.

## Reusable Library Sections

- [Knowledge library](knowledge/README.md): reusable project planning, Java modeling, RuoYi-Vue-Pro, and model-card notes.
- [Skill library](skills/README.md): reusable Codex skill structures and model-card templates.

## Conclusion

For `gin-vue-admin` itself, MySQL is still the lower-friction choice because its default configuration and ecosystem path are MySQL-first. `RuoYi-Vue-Pro` and many similar Java/Vue admin systems also sit naturally in the MySQL ecosystem. If the goal is to study projects where PostgreSQL is a natural center of gravity, the best references are Directus, Baserow, ToolJet, Hasura, Supabase, Teable, Payload CMS, and Strapi.

## PostgreSQL-Oriented Projects

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

## How To Read The PostgreSQL List

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

## MySQL-Oriented Admin Projects

These projects are closer to `gin-vue-admin` and `RuoYi-Vue-Pro`: traditional business admin scaffolds, RBAC systems, code generators, workflow modules, and enterprise CRUD platforms. Their default docs, ecosystem, or common production path usually make MySQL the simplest choice.

| Rank | Project | GitHub | Main Language | Positioning | MySQL Fit |
|---:|---|---|---|---|---|
| 1 | RuoYi-Vue-Pro | `YunaiV/ruoyi-vue-pro` | Java | Spring Boot + MyBatis Plus + Vue admin system with SaaS, workflow, CRM, ERP, MES, AI, IoT, payment, and mall modules | Very strong fit. MySQL is the most natural deployment path for this ecosystem. |
| 2 | gin-vue-admin | `flipped-aurora/gin-vue-admin` | Go | Gin + Vue3 admin and development platform with RBAC, dynamic routes, code generator, form generator, import/export, MCP, and AI-assisted features | Very strong fit. The default config is `db-type: mysql`. |
| 3 | RuoYi-Vue | `yangzongzhuan/RuoYi-Vue` | Java | Official RuoYi Spring Boot + Spring Security + JWT + Vue permission management system | Very strong fit. Classic MySQL-backed Chinese enterprise admin scaffold. |
| 4 | RuoYi-Cloud | `yangzongzhuan/RuoYi-Cloud` | Java | Official RuoYi Spring Cloud + Alibaba microservice permission management system | Strong fit. MySQL is the common operational baseline. |
| 5 | JeecgBoot | `jeecgboot/JeecgBoot` | Java | AI low-code and zero-code platform with code generation, online configuration, forms, workflow, knowledge base, and model integrations | Strong fit. Commonly deployed with MySQL in Java enterprise environments. |
| 6 | RuoYi-Vue-Plus | `dromara/RuoYi-Vue-Plus` | Java | Enhanced RuoYi-Vue rewrite with MyBatis-Plus, Undertow, Knife4j, Hutool, and enterprise improvements | Strong fit. Follows the RuoYi/MyBatis/MySQL mainstream path. |
| 7 | RuoYi-Cloud-Plus | `dromara/RuoYi-Cloud-Plus` | Java | Upgraded RuoYi-Cloud rewrite with Spring Cloud Alibaba, Dubbo, MQ, OSS, ES, Xxl-Job, and Docker | Strong fit. MySQL remains the default business data store in this stack family. |
| 8 | Pig | `pig-mesh/pig` | Java | Spring Cloud / Spring Boot / OAuth2 RBAC permission management platform | Strong fit. Typical Java microservice admin stack with MySQL-friendly schema and MyBatis-style ecosystem. |
| 9 | Guns | `stylefeng/Guns` | Java / Vue | Spring Boot 3 + Vue 3 enterprise application development framework | Strong fit. Traditional Java enterprise admin framework, naturally MySQL-friendly. |
| 10 | GoAdmin | `go-admin-team/go-admin` | Go | Gin + Vue / Element UI / Arco / Ant Design admin scaffold with RBAC, multi-tenant support, code generator, forms, and scheduled tasks | Strong fit. Similar to GVA in shape; MySQL is a straightforward production choice. |
| 11 | FastAdmin | `fastadminnet/fastadmin` | PHP / JavaScript | ThinkPHP + Bootstrap rapid admin framework with CRUD generation, menus, model/controller/view generation, and recycle bin | Strong fit. PHP admin ecosystem commonly uses MySQL. |

## How To Read The MySQL List

If you want projects closest to `gin-vue-admin` and `RuoYi-Vue-Pro`, start with:

1. RuoYi-Vue-Pro
2. gin-vue-admin
3. RuoYi-Vue
4. JeecgBoot
5. GoAdmin

If you want Java enterprise admin scaffolds, start with:

1. RuoYi-Vue-Pro
2. RuoYi-Vue
3. JeecgBoot
4. Pig
5. Guns

If you want Go admin scaffolds similar to GVA, start with:

1. gin-vue-admin
2. GoAdmin

## Notes

Appsmith and NocoDB are also well-known in the same general space, but they are better described as projects that can connect to PostgreSQL rather than projects whose own architecture is clearly PostgreSQL-first. They are useful references, but not first-choice examples for the PostgreSQL-specific question.

## Related Decision For Gin Vue Admin

For `gin-vue-admin`, the practical deployment recommendation remains:

- Use MySQL 8.x as the primary database.
- Keep Redis available for runtime cache/session/queue-style needs.
- Do not choose MongoDB just because the project has AI-assisted features.
- Consider PostgreSQL only if the surrounding product direction already benefits from PostgreSQL-specific capabilities.
