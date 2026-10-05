# Plages horaires recommandees pour redemarrer Home Assistant

## Objectif
Minimiser le risque d'interrompre une automatisation en cours (stores, lumieres, delais longs, TTS).

## Meilleures plages (faible risque)

- 02:30 a 05:45
- 00:15 a 01:30

## Heure recommandee par defaut

- 03:15

## Heures a eviter

- 06:20 a 09:20
- 11:55 a 12:10
- 13:10 a 13:40
- 15:25 a 15:40
- 16:50 a 17:40
- 18:00 a 23:10
- Toute fenetre autour de sunrise/sunset (environ +/- 90 min)

## Pourquoi ces plages

- Plusieurs automatisations importantes tournent le matin et le soir.
- Des sequences avec delay/repeat peuvent etre coupees par un reboot.
- Les automatisations basees sur le soleil varient selon la saison et augmentent le risque autour du lever/coucher.

## Checklist avant reboot

- Verifier qu'aucun store n'est en mouvement.
- Verifier qu'aucun script manuel de test n'est en cours.
- Eviter de redemarrer dans les 10 minutes avant une heure de trigger connue.

## Notes

- Un redemarrage peut interrompre une execution en cours, mais n'annule pas une commande deja envoyee a un appareil.
- Un trigger qui tombe pendant le downtime peut etre manque.
