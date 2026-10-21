# Inventaire — documentation marchand Woura

> Fichier de travail, exclu de la publication (`.mintignore`).
> Établi le 02/10/2026 sur `main` de `app`, `api`, `storefront`, `dashboard-storefront`, et `dev` de `woura-africa`.
> Colonnes : **Doc ?** = à documenter (oui / non / ? = question ouverte, voir en bas).

## A. Site public et compte (`app`, hors `(app)`)

| Route | Libellé exact | Règle métier (une phrase) | Doc ? |
|---|---|---|---|
| `/` | « Vends. Encaisse. Livre. Et sache enfin ce que tu gagnes. », « Commencer gratuitement », « Voir les tarifs », « Essai Gratuit » | Les deux CTA mènent à la connexion, pas à l'inscription. | oui (bref, dans Bienvenue) |
| `/pricing` | « Un abonnement clair. Des crédits pour le reste. » | Seule source publiable des prix. | oui (renvoi, prix vérifiés à l'écran) |
| `/auth/register` | « Créez votre compte » ; « Continuer avec Google » / « Continuer avec Facebook » ; Prénom, Nom, Email, Téléphone, Mot de passe, Confirmer ; « Créer mon compte » | Mot de passe : 8 caractères, majuscule, minuscule, chiffre, caractère spécial. | oui |
| `/auth/verify` | « Vérification du compte » — « Saisissez le code à 4 chiffres envoyé par email » ; « Vérifier », « Renvoyer le code » | Code à 4 chiffres par e-mail. | oui |
| `/auth/login` | « Bienvenue ! » ; « Se souvenir de moi », « Mot de passe oublié ? », « Se connecter » | — | oui |
| `/auth/forgot-password` | « Mot de passe oublié ? » ; « Envoyer le code de réinitialisation » | Le code arrive par e-mail, puis « Nouveau mot de passe ». | oui |
| `/contact` | « Contactez-nous » ; « Envoyer le message » | Canal support public. | oui (page Contacter le support) |
| `/legal/*` | CGU, Confidentialité, Cookies | — | non (renvoi seulement) |
| Accueil d'un nouveau compte | Étape 1 « Complétez votre profil » (Prénom, Nom, Numéro principal, Pays, Ville, « Continuer » ; sautée si le profil est complet) ; étape 2 « Votre boutique » : « Où vendez-vous aujourd'hui ? » (21/10) | Sans boutique, le menu latéral n'apparaît pas. Création rapide d'une boutique Woura, puis « Par quoi voulez-vous commencer ? » (facultative). | oui |

## B. Connecter ses boutiques (`app` : `/shops`, fenêtre d'ajout, `/link-shop`)

| Route | Libellé exact | Règle métier | Doc ? |
|---|---|---|---|
| `/shops` | « Mes boutiques » (ex « Ma boutique en ligne », V2) ; « Ajouter une boutique » ; « {count} / {total} boutiques » ; accès : nom de boutique → « Gérer mes boutiques », avatar, Toutes les fonctionnalités › Canaux de vente, Plus | Une seule boutique **active** à la fois ; tout le reste porte sur elle. `?ajouter=1` ouvre la fenêtre d'ajout. | oui |
| Fenêtre d'ajout | « Ajouter une boutique » : « Où vendez-vous aujourd'hui ? » → « Sur mon site Shopify, WooCommerce ou YouCan » / « Sur WhatsApp, TikTok, Facebook ou en live » (21/10) | Question affichée seulement si `WOURA_PUBLIC_STOREFRONT`. Sinon : « Reliez votre site Shopify, WooCommerce ou YouCan. » + lien WhatsApp « pour ouvrir votre boutique Woura ». | oui (variante avec drapeau) |
| Statut des plateformes | badge « Bientôt » / option masquée | Réglage staff par plateforme. En dev : Shopify, WooCommerce, YouCan, WhatsApp actifs ; Chariow « Bientôt ». | ? (Q1) |
| Champs communs | « Nom de la boutique », « Pays », « Devise principale », logo | Refus au-delà du plan : « Limite de boutiques atteinte pour votre abonnement. » | oui |
| Shopify | « Installer l'app Shopify », « Code unique » (XXXX-XXXX), « Lier la boutique » | Erreur : « Code invalide ou boutique déjà liée à un compte. » | oui |
| App Shopify (woura-africa) | « Connectez votre boutique », « Domaine », « Code unique » + « Copier », « Accéder au Dashboard Woura → » | **Sur `main` (déployé) le libellé est encore « Clé API »** ; « Code unique » n'est que sur `dev`. L'app s'ouvre hors de l'admin (`embedded = false`). | oui, avec Q6 |
| `/link-shop` | « Finaliser la connexion de votre boutique », « Préparation de la connexion de votre boutique… », « Lien invalide », « La connexion automatique n'a pas abouti… » | Selon connexion / nombre de boutiques : tableau de bord, onboarding ou `/shops` avec la fenêtre préremplie. | oui |
| Succès Shopify | « Boutique Shopify connectée ! » | — | oui |
| WooCommerce | « URL de la boutique », « Consumer Key », « Consumer Secret », « Connecter la boutique » | Produits importés à la connexion ; commandes antérieures **non** importées (il faut synchroniser). Erreur « Impossible de se connecter à WooCommerce: … ». | oui |
| YouCan | « Connecter avec YouCan » | Produits importés à la connexion ; seules les nouvelles commandes arrivent ; retours d'erreur : « Autorisation refusée », « La connexion a expiré », « Limite de boutiques atteinte », « YouCan n'a pas confirmé la connexion ». | oui |
| ~~Boutique WhatsApp~~ | Retirée le 03/10/2026 : plus de création de boutique WhatsApp ; l'assistant se relie à une boutique existante | — | retiré |
| Boutique Woura | « Créer ma boutique Woura » : Nom, Pays (devise affichée), WhatsApp facultatif → « Créer ma boutique » ; puis « Par quoi voulez-vous commencer ? » (6 choix + « Passer ») | Création rapide dans l'app (N5, `POST /shop/creation-rapide/`) ; la vitrine se crée ensuite dans le builder sur CETTE boutique (`/create?boutique=<id>`, « Créer la vitrine de {nom} », nom et pays figés, une seule génération). Seulement avec `WOURA_PUBLIC_STOREFRONT`. | oui |
| Autres boutiques | « Utiliser cette boutique », « Réactiver », « Prolonger », « Convertir en vitrine Woura » → « Confirmer la conversion » | « Convertir » n'apparaît que pour une boutique non Woura **et non active**. | oui |
| Synchro commandes (`/orders`) | « Actualiser » → fenêtre « Synchronisation », « Profondeur (jours) » (1–365, 14 par défaut), « Lancer la synchronisation » | Proposée pour toutes les plateformes sauf WhatsApp. | oui |
| Synchro produits (`/products`) | « Synchroniser avec {source} » → « Synchronisation Produits » | Shopify et WooCommerce seulement. | oui |
| Boutique indépendante | formulaire présent, inaccessible | — | non (masqué) |
| Déconnecter / supprimer une boutique (app) | — | Absent de l'application. | non → « contacter le support » |

## C. WhatsApp (`app` : `/whatsapp-shop`, `/whatsapp-campaigns`, `/settings`)

| Route | Libellé exact | Règle métier | Doc ? |
|---|---|---|---|
| `/whatsapp-shop` | Redirige vers l'assistant de la boutique active (création de boutique WhatsApp retirée le 03/10/2026) ; liaison : « Connecter WhatsApp Business », nom du chatbot (défaut « Sarah ») | Connexion Meta Embedded : admin du Business Manager, toutes les autorisations ; le marchand garde WhatsApp sur son téléphone. | oui |
| Mode OTP (BSP) | « Meta BSP (OTP) » | Drapeau `WHATSAPP_BSP` (100 % en dev). | ? (Q3) |
| `/whatsapp-shop/{id}` | « Assistant WhatsApp » (« Bêta ») ; onglets « Discussions », « Essayer », « Réglages », « Catalogue » ; « Mettre en ligne » / « Mettre hors ligne » | Menu derrière `AI_ASSISTANT` (pilote liste blanche en dev). Réponse : 2 crédits dans la fenêtre 24 h, 4 hors fenêtre. | ? (Q2) |
| Catalogue WhatsApp | « Relier votre catalogue WhatsApp », « Envoyer N produits » | Numéros Meta seulement ; produits sans photo non envoyés. | ? (Q2) |
| Comportements `WHATSAPP_FLOW_V2` | paiement dans la conversation, frais de livraison, mémoire client | Variable d'environnement (vraie en dev). | ? (Q2) |
| `/whatsapp-campaigns` | « Campagnes WhatsApp » ; « Nouvelle campagne » ; « Confirmer et envoyer » ; « Aucun modèle approuvé. Contactez Woura. » | Audience : a écrit ou commandé, opt-in, pas de STOP ; coût = destinataires × prix (3 crédits marketing, 1 utilitaire) ; drapeau `WHATSAPP_CAMPAGNES` au lancement. | ? (Q3) |
| Paramètres → Notifications → « WhatsApp Business » | « Numéro WhatsApp », « Activer les notifications WhatsApp », 8 alertes | « Nouvelle commande » désactivée par défaut (gratuite) ; autres alertes 2 crédits. | oui |
| Fenêtre de 24 h | — | Alertes en message libre dans les 24 h qui suivent un message du marchand ; sinon modèle payant ; message d'invitation chaque matin (7 h UTC = 8 h Cotonou) aux boutiques avec une commande la veille. | oui |
| Paramètres → « Prévenir vos clients » | « Suivi de commande » + messages acceptée / en livraison / livrée | Désactivé par défaut ; numéro du marchand, sinon Woura, sinon e-mail gratuit ; 1 crédit par WhatsApp. | oui |
| Déconnecter un numéro, zones WhatsApp, QR code | — | Absent. | non → support |

## D. Builder — Dashboard Storefront (`dashboard-storefront`)

| Route | Libellé exact | Règle métier | Doc ? |
|---|---|---|---|
| `/select` | « Sur quelle boutique souhaitez-vous travailler ? » ; « Continuer la configuration » / « Configurer cette boutique » ; « Créer une nouvelle boutique » | Sans boutique : redirection vers `/create`. | oui |
| `/create` | « Créer ma boutique » : Activité → Produits → Génération → Relecture ; « Générer ma boutique », « Publier ma boutique », « Personnaliser » | 1 à 5 produits, 1 à 5 photos (JPG/PNG/WebP, 5 Mo) ; **offert** ; 3 créations par jour (API, message « 3 créations par jour au plus… ») ; 13 pays. | oui |
| Barre d'outils | « Sauvegarder » (⌘S), « Publier », Brouillon / Publié / En pause, annuler/refaire | Sauvegarde auto toutes les 30 s. | oui |
| `setup` Identité | Nom, Accroche (tagline), Description, Logo, Bannière, « Sous-domaine de la boutique », WhatsApp, Pays, Devise | Sous-domaine verrouillé après publication. | oui |
| `theme` | « Thème & Couleurs » : Kora, Sahel, Lagune ; Couleur principale / secondaire | Choisir un thème remplace les couleurs. | oui |
| `products` | « Produits affichés sur la boutique » ; Affiché / Masqué ; « Gérer mes produits » | Aucun produit ne se crée ici. | oui |
| `cod` Formulaire | champs, « Ajouter un champ personnalisé », « Dans la page » / « Dans une modale » | Nom complet et Numéro WhatsApp fixes. | oui |
| `cod` Paiement | « À la livraison uniquement », « Paiement en ligne uniquement », « Au choix du client », Acompte, « Instructions de paiement », « Proposer ce moyen de paiement », « Demander la capture du paiement » | En ligne exige un sous-compte WouraPay, sinon repli à la livraison. | oui (en ligne : ? Q5) |
| `cod` Livraison | « Livraison gratuite » / « Tarif fixe » / « Par zone géographique » ; « Délai de livraison » ; « Zone de couverture » ; « Bloquer les commandes quand le stock est à 0 » | Défaut : stock non bloquant ; même réglage que Paramètres → Produits ; enregistré immédiatement. | oui |
| `cod` Messages | « Message de confirmation de commande » ; « Notification WhatsApp client » (« Coût : {n} crédit(s)… ») ; « Alerte WhatsApp marchand » | Confirmation : mode à la livraison seulement, modèle Meta validé. Correctif « Alerte WhatsApp marchand » = réglage de la boutique : **sur `dev` seulement**. | oui, avec Q6 |
| `offers` | « Offres de quantité » ; « Créer une offre » ; paliers, « Populaire » | Une offre = un produit. | oui |
| `upsell` | « Upsell & Downsell » ; « Créer une règle » ; « Après la commande » / « Avant le paiement » | — | oui |
| `codes-promo` | « Codes promo » ; « Nouveau code » ; Pourcentage / Montant fixe, Achat minimum, Utilisations max, Fin de validité | Remise sur les articles, jamais la livraison ; pas de modification d'un code (activer / supprimer seulement). | oui |
| `avis` | « Avis clients » ; « Masquer » → 4 motifs ; « Republier » ; « Répondre publiquement » | Seules les commandes livrées notent ; motif obligatoire. | oui |
| `pixel` | « Pixels & Tracking » : Facebook, TikTok, Google, Snap | — | oui |
| `policies` | Livraison / Retours / Conditions générales / Mentions légales ; « Régénérer avec l'IA » ; FAQ (20 max) ; À propos | Régénération limitée à 20 par heure, non facturée. | oui |
| `preview` | « Aperçu live » ; bureau / mobile ; « Ouvrir » | Montre le dernier brouillon enregistré. | oui |
| `publish` | « Prêt à publier ? » ; « Publier ma boutique » ; « Copier le lien », « Partager sur WhatsApp », « Mettre en pause la boutique » | Exige nom + formulaire COD. | oui |
| `settings` Domaine | « Domaine personnalisé » ; CNAME → `shops.woura.shop` ; « Vérifier le DNS » | Pas de domaine racine ; certificat automatique en ~10 min. | oui |
| `settings` SEO / Contacts / Boutique | Titre SEO, Meta description ; réseaux sociaux ; « Activer la protection » ; maintenance | — | oui |
| `settings` Zone de danger | « Supprimer cette boutique » → « Supprimer définitivement » | Existe dans le builder (contredit « pas de suppression » de la consigne). | ? (Q7) |

## E. Vitrine (`storefront`)

| Route | Libellé exact | Règle métier | Doc ? |
|---|---|---|---|
| `/` | « Nos produits », « Questions fréquentes » | Rayons dès 2 catégories. | oui |
| `/catalogue` | « Notre Catalogue », tri, « Filtres », « En stock uniquement » | 20 produits par page. | oui |
| `/p/{produit}` | badges « Promo », « Rupture de stock » ; « Commander maintenant » ; « Avis clients » | Formulaire seulement si des champs COD sont actifs. | oui |
| Formulaire | « Zone de livraison », « Vous avez un code promo ? », « Mode de paiement » : « Payer à la livraison » / « Payer {montant} maintenant » / « Payer par transfert » | — | oui |
| `/panier` | « Votre panier » | 20 articles différents, quantité 99 ; offres par quantité hors panier. | oui |
| `/commande/succes` | « Commande confirmée ! » ; « Montant à transférer » ; « Après le transfert, joignez la capture » | Détail visible seulement sur l'appareil qui a commandé. | oui |
| `/suivi` | « Suivre ma commande » ; « Numéro de commande » (WS-…), « Téléphone » | Étapes : Commande reçue, Confirmée, En livraison, Livrée. | oui |
| Avis | « Votre avis compte » ; « Publier mon avis » | Commande livrée + appareil qui a commandé. | oui |
| `/acces` | « Accès boutique » ; « Accéder à la boutique » | Mot de passe de boutique. | oui |
| `/contact`, `/policies`, `/a-propos` | « Nous contacter », « Nos Politiques », « À propos de {Nom} » | — | oui |

## F. Application marchand — gestion (`app/(app)`)

| Route | Libellé exact | Règle métier | Doc ? |
|---|---|---|---|
| `/dashboard` | Menu « Accueil » ; « Bienvenue, {name} » ; « À faire maintenant » (+ alertes 48 h, retours, stock faible, livreur) ; « Pour démarrer » (4 étapes, sans « boutique ») ; carte « Votre boutique en ligne » ; Encaissé / Dépenses / Bénéfice net / Panier moyen ; bouton « Analyse IA » → `/analytics` (ne lance rien) | Analyse IA unique, dans Chiffres (D8). | oui |
| `/products` | « Produits » ; « Créer manuellement » / « Générer une fiche avec l'IA » ; Tous / Actif / Brouillon / Archivé | — | oui |
| `/products/{id}` | « Détails » / « Gestion du stock » ; « Ajouter du stock », « Retirer du stock », « Ajouter une variante », « Prix de revient » | — | oui |
| `/orders` | « Commandes » ; « Toutes » / « Programmées » ; « Créer une commande » ; « Exporter » | — | oui |
| `/orders/{id}` | « Changer statut », « Assigner un livreur », « Modifier les articles », « Télécharger facture » ; statuts En attente, Acceptée, En livraison, Livrée, Programmée, Annulée, À rappeler, Injoignable, Retournée | Transitions fixées par l'écran ; Livrée / Annulée / Retournée figées côté serveur. | oui |
| Paiement par transfert | « Capture à vérifier », « Montant attendu », « Valider le paiement » / « Refuser » | Refus = le client doit refaire le transfert ; n'agit pas sur la livraison. | oui |
| Commande non gérable | « Non gérable » ; « Cette commande ne peut pas être gérée. Rechargez votre portefeuille… » | En crédit unique une commande coûte 0 crédit ; blocage seulement dans l'ancien modèle à quotas, jamais pour la vitrine. | oui, selon Q4 |
| `/live` | « Vente en Live » (Bêta) ; « Préparer mon live » ; « Démarrer le live » ; « Copier le lien » ; « Ajouter un produit » ; « Retirer le n° » ; « Terminer le live » ; « Bilan du live » ; « Voir les commandes du live » | Flag `WOURA_LIVE` en liste blanche (pilotes, branche `feat/woura-live`) ; plans Pro et Business ; assistant relié et en ligne ; numéro jamais redonné ; stock revérifié au « oui » ; commandes en attente sans réservation. | oui (`application/vente-en-live`, branche `docs/woura-live`, sans captures) |
| `/customers` | « Clients » | Consultation seulement. | oui |
| `/after-sales` | « Service Après-Vente » ; « Créer un ticket » ; En attente / Contacté / Clôturé | — | oui |
| `/delivery-men`, `/call-center` | « Livreurs et agents » ; « Ajouter un livreur » ; « Copier le lien d'accès » ; « Ajouter un agent » | Documenter l'invitation et l'assignation seulement. | oui (partiel) |
| `/expenses` | « Dépenses » ; « Ajouter une dépense » | — | oui |
| `/analytics` | « Chiffres » ; onglets « Bilan » · « Dépenses » · « Audience » ; « À retenir » ; « Lancer l'analyse IA · 20 crédits » ; solde insuffisant → « Acheter des crédits » | Audience seulement si vitrine publiée et pas en pause. | oui |
| `/audience` | « Audience de la boutique » | Boutiques Woura seulement. | oui (si Woura documenté) |
| `/automations` | Ancien hub « Outils IA » ; en V2 redirige vers `/tools#marketing` | Page doc `application/outils-ia` retirée, redirigée vers `application/fiches-produits-ia`. | non (retiré) |
| `/tools` | « Toutes les fonctionnalités » : rubriques Canaux de vente, Livraison et opérations, Argent, Marketing et automatisation, Compte ; section « Votre menu » (épingler, retirer, rétablir, objectif) | V2 seulement pour « Votre menu ». | oui (`application/toutes-les-fonctionnalites`) |
| `/ia/product-sheets` | « Fiches produits IA » ; « Publier sur ma boutique » | 20 crédits par fiche. | oui |
| `/ia/facebook-ads` | « Publicités Facebook » ; « Relier avec Facebook » | Visible sans drapeau, mais App Review Meta en cours. | non (consigne) |
| `/settings` | Général, Localisation, Produits, Notifications | — | oui |
| `/account/settings` | Profil, Abonnement / Crédits, Encaissements | — | oui |
| `/notifications` | « Notifications » | — | oui |
| `/affiliation` | « Programme d'affiliation » ; « Copier », « Partager sur WhatsApp », « Mes filleuls » | 10 % du premier abonnement payé du filleul, une seule fois, versé sur le portefeuille WouraPay après 3 jours. | oui |
| `/abonnement` | « Abonnement et crédits » ; « Recharger », « Acheter des crédits » ; Gratuit / Pro / Business | Visible seulement si crédit unique ouvert (`CREDIT_UNIQUE`, pilote en dev). | ? (Q4) |
| `/forfaits` | « Forfaits & Packs » | Ancien modèle (pay-as-you-go). | ? (Q4) |
| Recharge | « Recharger le portefeuille » ; minimum 5 000 | — | oui |
| Factures | — | Aucune facture d'abonnement ou de recharge téléchargeable. | non (page « Historique » à la place) |

## G. Non documenté exprès

| Élément | Raison |
|---|---|
| `/backoffice`, admin Django | staff (règle 6) |
| `/delivery/dashboard`, `/callcenter` | espaces livreur et agent (sauf l'invitation) |
| Approvisionnement (`/sourcing`), Idées produits (`/listed-products`), Script et voix off (`/ia/scripts-audio`), onglet « Script audio » d'une fiche IA | masqués exprès (encore accessibles par ⌘K ou URL) |
| Publicités Facebook | App Review Meta en cours |
| « Discutez avec Wou » (`WOU_ASSISTANT`) | drapeau |
| Boutique indépendante, QR code WhatsApp | inaccessible |

## Décisions de l'utilisateur (02/10/2026)

- Facturation : seul le modèle **en crédits** (Gratuit / Pro / Business, « Abonnement et crédits ») est documenté. « Forfaits & Packs » ne l'est pas.
- Boutique Woura (« Non, pas encore » + builder) : **ouverte à tous** en production.
- Assistant WhatsApp, `WHATSAPP_FLOW_V2`, Campagnes WhatsApp, mode OTP (BSP) : **actifs** en production → documentés.
- Correctifs restés sur `dev` (« Code unique » Shopify, « Alerte WhatsApp marchand ») : l'utilisateur les déploie → la doc décrit le comportement **corrigé**.

## Questions ouvertes (à trancher par l'utilisateur)

1. **Plateformes et boutique Woura en production** : Shopify, WooCommerce, YouCan et WhatsApp sont-ils tous « actifs » ? `WOURA_PUBLIC_STOREFRONT` est-il ouvert à tous, ou encore un pilote ? (Toute la partie « Boutique en ligne » en dépend.)
2. **Assistant WhatsApp** : `AI_ASSISTANT` est un pilote en dev ; ouvert à qui en prod ? `WHATSAPP_FLOW_V2` est-il vrai en prod ?
3. **Campagnes** (`WHATSAPP_CAMPAGNES`) et **mode OTP / BSP** (`WHATSAPP_BSP`) : ouverts en prod ?
4. **Facturation** : `CREDIT_UNIQUE` est un pilote en dev. En prod, un marchand standard voit-il « Abonnement et crédits » (Gratuit / Pro / Business) ou « Forfaits & Packs » ? Je propose de ne documenter que le modèle en crédits si c'est lui qui est ouvert.
5. **Paiement en ligne (WouraPay)** pour la vitrine : à documenter comme disponible ?
6. **Deux correctifs ne sont que sur `dev`** : « Code unique » dans l'app Shopify (prod affiche « Clé API ») et « Alerte WhatsApp marchand » du builder. Les déployez-vous avant publication, ou j'écris le libellé actuel ?
7. **Suppression d'une boutique Woura** : le builder offre « Supprimer cette boutique ». La documenter (avec un avertissement), ou renvoyer au support ?
8. **Comptes de test** : avez-vous une boutique de dev Shopify avec l'app, un WooCommerce, un YouCan, un compte Meta Business de test ? Sinon je décris les écrans externes sans capture.
9. **Prérequis WooCommerce** (HTTPS, permaliens, hébergeur qui laisse passer l'API REST) : confirmés ?

## Écarts déjà relevés (pour le rapport final)

- **Sécurité** : `woura-africa/app/routes/debug.tsx` sans authentification renvoie `DATABASE_URL` et les codes uniques des boutiques (tâche séparée proposée).
- La consigne dit « chaque commande reçue consomme des crédits » : le code dit **0 crédit**. La fenêtre de synchronisation affiche pourtant « Les nouvelles commandes importées seront déduites de votre quota d'abonnement. » dans tous les modes.
- YouCan : les webhooks `order.updated` / `order.cancel` écrivent un mauvais champ (bug probable) ; un seul webhook enregistré (`order.create`).
- Le builder n'affiche aucun avertissement « aucun numéro ne reçoit les alertes » (correctif sur `dev` seulement).
- Builder : bannière bloquée à 2 Mo malgré « Max 5 MB » ; « Au moins 1 produit visible » affiché obligatoire mais non vérifié ; « Pixel Facebook configuré » lit un champ obsolète ; deux listes de pays différentes ; « FCFA » codé en dur dans Offres et Upsell ; politiques par défaut « paiement exclusivement à la livraison » ; fautes « à accepté », « sold », « creme ».
- Vitrine : message technique « Next ne peut pas joindre le backend configuré dans .env.local » visible en prod ; ancres du pied de page vers les politiques ignorées ; libellés de paiement différents entre confirmation et suivi.
- App Shopify : textes anglais, erreur API brute (« Veuillez vérifier les logs »), lien « Accéder au Dashboard Woura → » vers le site et non le tableau de bord.
- Application : clés i18n manquantes (`Orders.detail.downloadError`, `DeliveryMen.errors.loadSections`, `Account.affiliation.wourapayPortalError`…), validations anglaises sur `/contact`, « Essai Gratuit » mène à la connexion, « WordPress » au lieu de WooCommerce dans « Prévenir vos clients », textes d'onboarding inversés, tutoiement / vouvoiement mélangés, source « YOUCAN » / « WOURA » brute dans la fenêtre de synchro.
- API : 6 contradictions entre `docs_content` et le code (affiliation par défaut, plafond retiré, notifieur, garde des campagnes, décompte des drapeaux, fuseau des messages du matin).

## Écarts relevés pendant la rédaction (lots 1 et 2)

- **Assistant WhatsApp — réglages ignorés** : horaires et message d'absence, photos, vocaux, mots et message d'escalade, protection anti-abus sont envoyés par l'écran mais absents des serializers de l'API (`whatsapp_shop/serializers.py`) : DRF les jette sans erreur, tout revient au défaut au rechargement. Non documentés ; tâche de correction proposée.
- **Connexion Meta = assistant en ligne immédiatement** (`whatsapp_shop/api.py` création et rattachement : `is_live=True`), alors que l'étape « Tester » dit « avant de le mettre en production ». La doc avertit.
- **Alertes marchand hors fenêtre 24 h : non envoyées** (`notification/whatsapp_service.py`, seuls les messages d'ouverture partent en modèle). La consigne disait « hors fenêtre, modèle payant ». L'aide du champ `wa_window_opened_at` dit « gratuit » en fenêtre, mais le code facture tout sauf « Nouvelle commande ».
- **Bandeau « WhatsApp Pro » (page Commandes)** : son texte parle des messages de suivi des clients, mais il reflète la fenêtre du marchand.
- **Synchronisation manuelle** : refusée pour une boutique de type WOURA (native ou convertie : « n'est pas disponible pour les boutiques de type 'WOURA' »), mais **Actualiser** reste affiché ; la fenêtre affiche la source brute « WOURA » / « YOUCAN » et la note « déduites de votre quota d'abonnement » même en mode crédits.
- **Campagnes** : les cartes de modèles affichent le texte anglais même langue FR choisie ; les champs du contenu portent les noms techniques (`nom_client`, `date_fin`) ; tous les modèles marketing exigent une image (téléversement obligatoire).
- **Prix des plans** : la base de dev affiche Pro 10 000 F / Business 20 000 F (1 mois), la migration de départ 20 000 / 40 000. La doc ne fige aucun prix ; à vérifier sur la page Tarifs de prod.
- **Textes** : écran vide de « Ma boutique en ligne » promet « 1 000 F de crédits » (bonus désactivé, `WALLET_WELCOME_BONUS=0`) ; titre de fenêtre « Ajouter la boutique » vs bouton « Ajouter une boutique » ; accueil : « Woocommerce », exemple de ville « Abidjan » pour un compte au Bénin, textes des étapes 1/2 inversés dans le code ; « Prévenir vos clients » cite « WordPress » ; vitrine en « XOF », application en « FCFA ».
- **Shopify** : aide « code affiché dans votre admin Shopify » alors que l'app s'ouvre hors de l'admin (`embedded = false`) ; bouton « Voir le tutoriel vidéo » jamais affiché (URL non configurée).
- **Vitrine de dev :3002** : rechargements en boucle (Turbopack « stale », socket HMR fermé) → quotas DRF par IP épuisés ; captures faites sur une copie webpack :3012.
- **Mintlify** : CLI refusée sous Node 25 ; Node 22 téléchargé dans le scratchpad (avec accord).

## Écarts relevés (lot 3 — boutique en ligne)

- **Message de confirmation** : l'éditeur annonce les variables `{nom}`, `{produit}`, `{montant}`, mais ni la vitrine (`storefront/lib/utils.ts` `getCodConfirmationMessage`) ni l'API ne les remplacent : le client les voit brutes. La doc conseille un message sans variable.
- **Variante « Default »** affichée au client (confirmation, suivi) pour un produit sans variante.
- **Éditeur, nom de boutique** : le sélecteur lit la boutique mémorisée (`woura-shop-switcher` en localStorage), pas celle de l'URL : un lien direct vers `/builder/<id>` peut afficher le nom d'une autre boutique.
- **Aperçu de l'éditeur en dev** : `NEXT_PUBLIC_SHOP_PREVIEW_BASE` pointe sur `localhost:3000` (l'application) et `frame-src` n'autorise que `:3000` → aperçu vide en local. Captures faites sur une copie (:3011 → vitrine :3012).
- **Bannière** : aide « Max 5 MB » mais refus au-delà de 2 Mo (déjà relevé) ; documenté en Warning.
- **Modèles de politiques** : « paiement exclusivement à la livraison » ; documenté en Warning.
- **Offres / Upsell** : « FCFA » codé en dur dans l'éditeur ; documenté en Note.
- **Champ « Code promo » du formulaire** : le lien « Vous avez un code promo ? » s'affiche toujours dans la vitrine, indépendamment de ce champ.
- **Domaine de démo** : les captures montrent l'adresse fictive `maison-awa.woura.shop` (sous-domaine inexistant du domaine de prod) — à valider.

## Écarts relevés (lots 4 à 6 — application, abonnement, aide)

- **Fiches produits IA** : la fenêtre annonce que l'IA rédige « arguments, avis, FAQ » → de faux avis clients peuvent être publiés sur une fiche. La doc demande de les retirer avant publication (déjà « à trancher » côté produit).
- **SAV** : les cartes affichent « Livré le - » sans date pour des commandes livrées.
- **Assigner un livreur** : « Frais de livraison » pré-rempli à 1 500 alors que les frais par défaut de la boutique de démo valent 1 000.
- **Chiffres** : la flèche du Profit est rouge (« ↘ 138 % ») alors que le profit augmente ; l'agenda « Programmées » n'affiche pas la commande programmée de démo.
- **Audience** : taux de conversion > 100 % quand des commandes précèdent la mesure des visites (expliqué en Note).
- **Compte** : l'onglet « Prix d'import » reste visible (fonction « Idées produits » masquée) ; la complétion du profil compte « le numéro de l'assistant » (Wou).
- **Factures** : aucune facture d'abonnement ou de recharge téléchargeable (documenté en Note).
- **Affiliation** : le parrainage passe par un lien `?ref=` et un cookie : un filleul qui s'inscrit sur un autre appareil n'est pas rattaché.
- **Onglets du compte selon le mode de facturation global** : en dev (`pay_as_you_go`) on voit « Crédits et consommation », « Historique », « Tarifs des services » ; à vérifier en production.

## H. Navigation V2 (lot N7, 21/10/2026, branche `docs/navigation-v2`)

Sources lues : `app` dev (2ee0a7c), `hooks/use-menu-marchand.ts` (`menuV2`, `raccourcis`, `rubriquesDuCatalogue`), `components/admin/{admin-sidebar,mobile-bottom-nav,feuille-plus,command-palette,admin-navbar}.tsx`, `app/[locale]/(app)/{tools,dashboard,analytics}/page.tsx`, `components/dashboard/*`, `components/shops/{add-shop-modal,objectif-demarrage,active-shop-banner}.tsx`, `messages/fr.json` ; `api` dev `shop/signaux_navigation.py`, `shop/alertes.py` ; `dashboard-storefront` dev `create/page.tsx`.

### Ce qui a changé dans la doc

- Nouvelle page `application/toutes-les-fonctionnalites.mdx` (groupe « Se repérer », avec l'accueil).
- `application/outils-ia.mdx` → `application/fiches-produits-ia.mdx` (`git mv`), redirection Mintlify dans `docs.json`.
- `application/tableau-de-bord.mdx` titrée « L'accueil » (adresse inchangée : `lib/aide.ts` de l'app pointe dessus).
- Tous les chemins « menu X » réécrits pour la V2, ordinateur et téléphone ; création de boutique Woura rapide + vitrine facultative ensuite ; WooCommerce : import des 14 derniers jours ; assistant : onglet Résultats ; Chiffres : onglets, À retenir, analyse IA unique.
- 53 emplacements de captures retirés (ancien menu ou écran changé), 12 nouvelles captures à faire : `_captures-a-refaire.md`.

### Écarts code / interface relevés

- ~~**Résultats de l'assistant** : la note disait « Tester » au lieu d'« Essayer »~~ : corrigé sur `dev` (recette N7).
- ~~**Checklist** : `copyError` renvoyait vers « Ma boutique en ligne » ; clés mortes~~ : corrigé sur `dev` (recette N7).
- ~~**Téléphone : personnaliser le menu difficile à trouver**~~ : la feuille « Plus » a maintenant « Personnaliser le menu » (recette N7) ; la doc donne ce chemin.
- **Sans aucun raccourci**, le groupe « Vos raccourcis » et son lien « Personnaliser » disparaissent (`menuV2` filtre les groupes vides) : seule la section « Votre menu » de `/tools` permet d'épingler.
- **`lib/aide.ts` (app)** : `/ia` et `/automations` pointent sur `/application/outils-ia` (la redirection les rattrape) ; `/tools` et `/live` n'ont pas de page d'aide (le « ? » mène à l'accueil de la doc). À ajouter : `/tools` → `/application/toutes-les-fonctionnalites`, `/live` → `/application/vente-en-live`, `/ia` → `/application/fiches-produits-ia`.
- ~~**Vitrine d'une boutique existante** : l'assistant redemandait des produits, doublons possibles~~ : **corrigé sur les branches `fix/vitrine-boutique-existante`** (api 096c2664, builder 37c9c5e, non fusionnées). Quand la boutique a déjà des produits, l'étape « Vos produits » est facultative (ils sont tous sur la boutique en ligne, tels quels) et un produit déjà au catalogue (même nom) est refusé. La doc décrit ce comportement.
- **Builder, étape Activité** : plus de « Frais de livraison » (commit 1819a47) ; la doc et la capture `01-activite.png` le montraient.
- **Commandes** : « Toutes / Programmées » est devenu « Liste / Calendrier » ; bouton « Service après-vente » dans l'en-tête (icône seule sous 640 px).
- **Indicateurs** : « Chiffre d'affaires / Profit » → « Encaissé / Bénéfice net » (accueil et Chiffres) ; titre « Mes Chiffres » → « Chiffres » ; « Service Après-Vente » → « Service après-vente ».
- **Conversion en vitrine Woura** : proposée aussi sur la carte de la boutique active (la doc disait « jamais la boutique active »). D11 (retrait pour WooCommerce/YouCan, branches `fix/conversion-boutique-integree`) non fusionné : la section sera à revoir.
- **Accès des livreurs et agents (corrigé dans la doc le 21/10)** : le chemin est l'app mobile Woura Agent, accès par e-mail.
  - Sur `dev`, créer un livreur ou un agent ne lui envoyait **aucun identifiant**. **Branches `feat/acces-a-la-creation`** (api b4cb3d15, app 1327f3d, non fusionnées) : un livreur « Personnel » (TERTIARY) ou un agent créé par le marchand reçoit ses accès **aussitôt** (`envoyer_acces_a_la_creation`, même chemin et mêmes garde-fous que le bouton) ; le message après l'ajout dit ce qui est parti ; un échec n'empêche pas la création. La doc décrit ce comportement. ~~Écarts : message renvoyant au bouton absent quand l'application n'est pas publiée ; e-mail seul cité quand les deux canaux sont partis~~ : corrigés sur la même branche (app 37712d7, clés `accesAppPasPubliee` et `accesEmailEtWhatsapp`), la doc suit.
  - Action visible seulement pour un livreur TERTIARY (« Personnel ») ou un call center PRIVÉ, et seulement si au moins un lien de store est renseigné.
  - Le bouton **« Copier le lien d'accès »** existe encore sur la fiche d'un livreur non PRIMARY (`delivery-men/[slug]/page.tsx`, `generate_access_link`) ; il mène au tableau de bord web livreur, qui affiche une fenêtre poussant l'app. La doc ne le décrit plus. Aucun bouton équivalent côté agents.
  - Nom de l'app incohérent : « Woura Agent » sur iPhone (`CFBundleDisplayName`), « Woura Agents » sur Android (`android:label`) et dans l'app ; l'e-mail dit « l'application Woura ».
- **Commentaire de tête de `use-menu-marchand.ts`** : décrit encore la V1 (« Vendre, Livrer, Ma boutique, Argent, Automatiser »).
- **Copie de travail de `app`** : une modification non commitée (autre session) retire la carte « Veille marché » du catalogue.
- **Bouton « Abonnement »** de l'en-tête : masqué sous 400 px, il passe dans le menu de l'avatar.

### Questions ouvertes (navigation V2)

10. **`NAVIGATION_V2` est en liste blanche** : la doc décrit la V2 pour tout le monde. La publier seulement à l'ouverture à tous, ou ajouter une note pour les comptes encore en V1 ?
11. **Cartes non documentées dans « Toutes les fonctionnalités »** : Approvisionnement (Livraison et opérations), Publicités Facebook (Marketing et automatisation, et raccourci quand un compte publicitaire est relié), Veille marché (`DISCOVERY`), Wou (`WOU_ASSISTANT`). La doc ne les cite pas : les documenter, ou les masquer dans l'app ?
12. **Emplacement de la nouvelle page** : groupe « Se repérer » en tête de l'onglet Application (avec l'accueil), plutôt que « Outils et réglages ».
13. **Anciennes images** restées dans `images/` (dont `images/application/outils-ia/`) : à supprimer une fois les captures refaites ?

### Quatre fonctions documentées (décision du 21/10/2026)

Pages : `application/approvisionnement`, `application/publicites-facebook`, `application/veille-marche`, `connecter/wou`. Sources : `app` dev (a482f5b), `api` dev (7e8bf54d). `dev` de l'app a aussi avancé (recette N7) : « Personnaliser le menu » dans Plus, badge « Pilote » sur Campagnes WhatsApp, action « Préparer un live » dans la recherche, pastille « Abonnement » en icône seule sur téléphone ; la page « Le menu et toutes les fonctionnalités » est à jour.

- **Approvisionnement** : création gratuite (aucun débit), photo du colis et bon de livraison obligatoires ; « Valider » crée une dépense APPROVISIONNEMENT (montant total, date de commande) et une entrée de stock ; le prix de revient du produit n'est pas touché. « Modifier » reste affiché sur un approvisionnement validé ou annulé, mais l'API refuse. Supprimer un validé retire la dépense, pas le stock. Libellés de champs non utilisés dans le formulaire (mode d'expédition, dimensions, coût par m³).
- **Publicités Facebook** : lecture seule dans l'interface (aucune création ni pause de campagne), autorisations demandées plus larges (`ads_management`). Analyse : `fb_ads_ai_analysis`, 60 crédits dans la grille de départ (`wallet/0022`), coût affiché par le widget au-dessus du bouton. Connexion liée au compte, pas à la boutique. L'analyse « en direct chez Meta » dépend d'un réglage serveur et d'une autorisation supplémentaire de Meta. Les dépenses publicitaires ne sont pas ajoutées aux Dépenses.
- **Veille marché** : la carte est dans `dev` commité (`DISCOVERY`) ; une modification **non commitée** de la copie de travail de `app/` (autre session) la retire du catalogue. Titre de page « Veille Marché » (majuscule) contre « Veille marché » ailleurs ; badges « Active », « Inactive », « Testing » codés en anglais. Sauvegarder (`discovery_save`) : 0 crédit en crédits, mini fiche offerte ; fiche complète = `product_sheet`. Aucun moyen de retirer une sauvegarde dans l'interface (clé `Discovery.saved.unsave` inutilisée). La fiche complète reprend l'image de la publicité d'un autre vendeur. Non documentés : l'onglet « Prix d'import » de Mon compte et `/listed-products/<id>` (anciennes « Idées produits »).
- **Wou** : l'API ne lit pas le drapeau `WOU_ASSISTANT` (seulement `owner_assistant_enabled`, mis à vrai dès qu'un numéro d'assistant est enregistré) ; le drapeau ne fait que masquer la section du compte et les cartes. Aucun débit de crédits. Rapport du lundi matin (crontab 7 h, prévu pour 8 h à Cotonou) seulement si la fenêtre de 24 h de la boutique est ouverte. La carte « Wou » du catalogue mène à `/account/settings` sans l'ancre `#wou` (la carte de l'accueil, elle, l'a).
- **`lib/aide.ts`** : `/ia/facebook-ads` tombe sous le préfixe `/ia` → page Fiches produits IA ; `/sourcing` et `/listed-products` n'ont pas de page d'aide. À ajouter : `/sourcing` → `/application/approvisionnement`, `/ia/facebook-ads` → `/application/publicites-facebook` (avant `/ia`), `/listed-products` → `/application/veille-marche`.
- ~~**Écart plus large** : pages qui parlaient d'onglets d'argent dans Mon compte~~ : corrigé dans la doc (21/10). `abonnement/historique` devient « Consommation, historique et encaissements » et reçoit la section Encaissements ; `application/compte` ne décrit plus que le profil et renvoie vers **Abonnement et crédits** (onglets `abonnement`, `consommation`, `historique`, `tarifs`, `encaissements` ; anciens `?tab=` redirigés). Mises à jour : `plans-et-credits`, `affiliation`, FAQ de `credits-et-abonnement`, `commande-et-livraison`, carte de `toutes-les-fonctionnalites`. La capture des encaissements change de chemin : `images/abonnement/historique/04-encaissements.png`.
- **Cloche** : « Tout marquer comme lu » n'existe pas dans le menu de la cloche ; retiré de `application/notifications`.
- **Titres alignés (branche app `feat/titres-alignes`, 829055d, non fusionnée)** : Paramètres, Livreurs et agents, Affiliation, Publicités Facebook et Veille marché ont pour titre le nom du menu ; sur Publicités Facebook sans compte relié, « Relier votre compte publicitaire Facebook » devient un sous-titre. La doc suit : titres de pages `application/parametres` (« Paramètres »), `abonnement/affiliation` (« Affiliation »), `application/livreurs-et-agents` (« Livreurs et agents ») et liens qui les citaient ; « Veille Marché » retiré. Sur `dev`, les anciens titres restent affichés jusqu'à la fusion.

