# Plan d'application mobile IA: Robot humoriste en temps réel

## Vision
Créer une application mobile où un robot virtuel discute en direct avec des personnes et génère des vidéos IA humoristiques. Le but produit est de faire rire les utilisateurs avec des réactions spontanées, des sketches courts et une interaction naturelle.

## Fonctionnalités clés
1. **Conversation vocale en temps réel**
   - Entrée micro en streaming depuis iOS/Android.
   - Transcription live (Speech-to-Text).
   - Réponse du robot avec voix synthétique expressive (Text-to-Speech).

2. **Génération vidéo IA humoristique**
   - Création de clips courts (10-45 secondes) à partir du dialogue.
   - Avatar robot animé (lip-sync + gestes).
   - Styles de rendu multiples (cartoon, 3D, cyberpunk, etc.).

3. **Moteur “faire rire”**
   - Détection du contexte de conversation et du ton.
   - Génération de blagues adaptées (sans contenu offensant).
   - Notation de performance (taux de rire, likes, rewatch).

4. **Partage social**
   - Export vertical (9:16) optimisé pour Reels/TikTok/Shorts.
   - Galerie personnelle de vidéos générées.

## Stack technique recommandée

### Mobile
- **React Native + Expo** pour livrer iOS/Android rapidement.
- Alternatives natives si besoin de perf extrême: Swift/Kotlin.

### Temps réel audio
- **WebRTC** pour flux faible latence.
- **Amazon Chime SDK** ou couche WebSocket custom pour le transport.

### IA conversationnelle
- LLM temps réel (dialogue + humour + garde-fous).
- Prompt system avec persona robot + politiques de sécurité.
- Mémoire courte de session + profil utilisateur (opt-in).

### Voix
- **Amazon Transcribe** (STT) pour transcription live.
- **Amazon Polly** (TTS) ou modèle neural TTS spécialisé pour voix plus comique.

### Vidéo IA
- Pipeline asynchrone:
  1. Script comique (texte)
  2. Storyboard rapide
  3. Génération visuelle (modèle vidéo/animation)
  4. Lip-sync + montage + sous-titres
- Exécution en jobs via Lambda + files (SQS).

### Backend (aligné avec ce template)
- **AWS Amplify Gen 2** pour infra-as-code.
- **Auth**: Amazon Cognito.
- **API**: AppSync GraphQL (sessions, vidéos, scores, historique).
- **Data**: DynamoDB (profils, interactions, analytics).
- **Storage**: S3 (audio/video/assets).

## Architecture logique (MVP)
1. Mobile envoie audio live.
2. Backend transcrit et produit la réponse texte.
3. Backend synthétise la voix du robot et renvoie l'audio.
4. En parallèle, un job vidéo est déclenché.
5. Le client reçoit l'URL de la vidéo finale quand le rendu est prêt.

## Bibliothèques “avancées” utiles
- **LiveKit / WebRTC SDK**: communication audio/vidéo temps réel.
- **FFmpeg**: composition et post-traitement vidéo.
- **MediaPipe / face landmark libs**: animation faciale et suivi d'expressions.
- **Rive / Lottie**: animations UI et avatar léger en temps réel.
- **ONNX Runtime Mobile**: inférence locale partielle (ex: effets).

## Roadmap en 3 phases

### Phase 1 (4-6 semaines): Prototype conversation
- Audio live + transcription + réponse vocale robot.
- 1 persona humoristique stable.
- Historique de sessions.

### Phase 2 (6-8 semaines): Génération vidéo
- Génération de clips courts post-conversation.
- Sous-titres automatiques.
- Partage social.

### Phase 3 (8-12 semaines): Optimisation “rire”
- Personnalisation par type d'humour.
- A/B testing des prompts.
- Tableau de bord analytics (rétention, taux de rire, conversion).

## Points de vigilance
- **Sécurité du contenu**: modération stricte de l'humour.
- **Latence**: objectif < 1,5s pour impression “temps réel”.
- **Coût**: la vidéo IA est coûteuse; utiliser quotas/crédits par utilisateur.
- **Vie privée**: consentement explicite pour audio, visage, partage.

## KPI de succès
- Temps de réponse vocal moyen.
- Pourcentage de sessions avec partage vidéo.
- Taux de rétention J1/J7.
- Score de satisfaction utilisateur sur “drôle”.
