# Woura — Guide marchand : consignes pour les agents

Documentation utilisateur de Woura, publiée avec [Mintlify](https://mintlify.com).
Pages en MDX avec frontmatter YAML ; configuration dans `docs.json` ;
prévisualisation `npx mint dev` ; liens `npx mint broken-links`.
La CLI Mintlify refuse Node 25 : utiliser une Node LTS (20 ou 22).

## Le lecteur

Un marchand ordinaire qui vend sur Facebook, TikTok ou WhatsApp, souvent depuis
son téléphone. Il n'est pas technicien et ne connaît pas le jargon e-commerce.
Chaque page doit lui permettre d'agir seul.

## Les quatre espaces de Woura (vocabulaire à employer)

| Dans la doc, on dit | C'est | Dépôt source |
|---|---|---|
| **le site Woura** | woura.africa : présentation, Tarifs, inscription, connexion | `app/` hors `(app)` |
| **l'application Woura** (ou « votre espace ») | le back-office du marchand : commandes, produits, clients, livreurs, chiffres, WhatsApp, abonnement | `app/app/[locale]/(app)` |
| **l'éditeur de boutique** (Dashboard Storefront) | où l'on crée et règle sa boutique en ligne Woura | `dashboard-storefront/` |
| **votre boutique en ligne** (vitrine) | ce que voient les clients du marchand | `storefront/` |

L'application Shopify s'appelle **« Woura Africa »** (dépôt `woura-africa/`).

## Terminologie

- « boutique active » : la boutique sur laquelle portent commandes, produits et chiffres.
- Navigation (V2) : le menu a cinq entrées fixes, **Accueil** (et non « Tableau de bord »), **Commandes**, **Produits**, **Clients**, **Chiffres** ; puis **Vos raccourcis**, **Toutes les fonctionnalités**, **Paramètres**, **Centre d'aide**. Sur téléphone : barre du bas et **Plus** (plus de ☰). On écrit « dans **Toutes les fonctionnalités**, rubrique **…** », jamais « menu **Outils IA** » ni « menu **Ma boutique en ligne** » (devenu **Mes boutiques**). Toujours donner le chemin ordinateur ET téléphone. Page de référence : `application/toutes-les-fonctionnalites.mdx`.
- « crédit » : 1 crédit = 5 F CFA. Ne jamais écrire un prix sans l'avoir vu sur la page Tarifs locale.
- « Code unique » (Shopify), « Consumer Key » / « Consumer Secret » (WooCommerce) : libellés exacts de l'interface.
- Livreurs et agents : ils travaillent sur l'application mobile **Woura Agent** (Android et iPhone) et reçoivent leurs accès par e-mail avec **Envoyer les accès à l'application**. Ne plus présenter le lien d'accès comme le chemin, ne jamais écrire « sans installation » ni « rien à installer ».
- « paiement à la livraison » (et non « COD » seul) ; « COD & Livraison » seulement pour nommer le menu.
- « paiement par transfert » : Mobile Money ou virement, preuve vérifiée par le marchand.
- Statuts de commande côté marchand : En attente, Acceptée, En livraison, Livrée, Programmée, Annulée, À rappeler, Injoignable, Retournée.
  Côté client (suivi) : Commande reçue, Confirmée, En livraison, Livrée (ou annulée).
- Tout terme technique inévitable est expliqué à sa première apparition et ajouté à `demarrer/glossaire.mdx`.

## Style

- Français, vouvoiement, phrases courtes, une idée par phrase.
- Le **pourquoi** en une phrase avant le **comment**.
- Libellés d'interface **en gras** et **mot pour mot** (vérifiés dans `messages/fr.json` ET à l'écran).
- Montants : `10 000 F CFA` (espace insécable).
- Titres en casse de phrase.
- Gabarit d'une page de tâche : intro → `<Info>` prérequis → « Avant de commencer » → `<Steps>` → « Ce que voit votre client » → « Questions fréquentes » (`AccordionGroup`) → « Et ensuite ? » (`CardGroup cols={2}`).
- `<Warning>` pour tout ce qui coûte de l'argent ou ne s'annule pas (crédits, publication, suppression, paiement validé).

## Captures d'écran

- Uniquement sur les serveurs locaux, avec le compte de démonstration « Maison Awa ». Jamais de donnée réelle, de jeton, de clé ou de domaine de production à l'image.
- Bureau 1440×900, mobile 390×844, PNG < 500 Ko.
- Chemin : `images/<onglet>/<page>/<nn>-<etape>.png`.
- Toujours dans `<Frame caption="…">` avec un `alt` descriptif ; pas d'annotation dessinée.
- Ne jamais déclencher une action qui envoie (message, e-mail, paiement, génération IA) pour obtenir un écran : capturer avant le clic final.

## Frontières de contenu

Ne pas documenter :
- le back-office staff (`/backoffice`, admin Django) ;
- les espaces livreur (`/delivery/dashboard`) et centre d'appels (`/callcenter`) — seulement comment le marchand **invite** et **assigne** ;
- Script et voix off (et l'onglet « Script audio » des fiches IA) ;
- l'onglet « Prix d'import » de **Mon compte** et les anciennes fiches « Idées produits » (`/listed-products/<id>`), restes de l'ancienne fonction, à trancher par l'utilisateur ;
- la « Boutique indépendante », la connexion WhatsApp par QR code ;
- les noms de variables d'environnement, URL internes, détails d'architecture.

Décision du 21/10/2026 : **Approvisionnement**, **Publicités Facebook**, **Veille marché** et **Wou** sont documentés (`application/approvisionnement`, `application/publicites-facebook`, `application/veille-marche`, `connecter/wou`). Ce qui dépend d'un drapeau s'écrit « si la fonction est ouverte pour votre compte », sans nommer le drapeau ; pour Publicités Facebook, la connexion dépend d'une validation de Meta : le dire sobrement.

Ce que l'interface n'offre pas (déconnecter une boutique ou un numéro WhatsApp, zones de livraison WhatsApp) : écrire « contactez le support ».

## Sources de vérité

1. Les menus : `app/hooks/use-menu-marchand.ts` (`menuV2`, `raccourcis`, `rubriquesDuCatalogue` : menu, raccourcis et cartes de Toutes les fonctionnalités, lus par toutes les surfaces), `app/components/admin/feuille-plus.tsx` et `mobile-bottom-nav.tsx` (téléphone), règles des raccourcis dans `api/app/shop/signaux_navigation.py` ; `app/components/admin/admin-sidebar.tsx` n'est plus que le rendu. Éditeur : `dashboard-storefront/components/builder/BuilderSidebar.tsx`. Vitrine : `storefront/components/shop/`.
2. Les libellés : `messages/fr.json` de `app` et de `dashboard-storefront`.
3. Les règles : l'API Django `api/` (statuts `app/shop/statuts.py`, vitrine `app/shop/vitrine.py`, paiement `app/shop/public_views.py`, crédits `app/wallet/`).
4. `api/app/backoffice/docs_content/*.md` pour comprendre, jamais pour copier.

L'inventaire de travail est dans `_inventaire.md` (non publié). Les captures retirées ou à faire sont listées dans `_captures-a-refaire.md` (non publié) ; dans les pages, leur place est marquée par `{/* capture à refaire : images/… */}`.
