Le code modifié est dans le fichier driver_sx127x/main.c

par rapport au code de base quelques elements on été ajoutées:

une structure contenant le header et le message payload à envoyer pour communiquer:
typedef struct {
	uint8_t to;
	uint8_t from;
	uint8_t hop;
	uint8_t hoplim;
	uint8_t role;
	uint8_t msg_type;
	uint32_t msg_lenght;
	uint8_t message[254];
} payload_struct;
On s'assure avec les types uint8_t et uint32_t que les éléments sont bien de la bonne taille (1 octets pour uint8_t et 4 pour uint32_t)

Pour envoyer un message, on a modifier la fonciton send_cmd. On commence par remplir la structure décrite précédemment : le to avec le deuxième arguments de la commande et le reste est codé en dur car reste fixe peut importe le message. Si le message est vide, le type du message est un ack : type 1 sinon c'est un message de type texte.

Pour la réception du message, on a modifier la fonction _event_cb. Il suffit de rajouter cette ligne memcpy(&pld, message,sizeof(payload_struct)); pour donner la bonne forme au message. On peut ensuit vérifier si le message provient d'un routeur et si il nous ais destiner avent de l'afficher avec un simple if (non implémenté ici).

Les dernières modifications sont dans la fonction init_sx1272_cmd. L'ajout de ces lignes permets de configurer directement le channel lora avec les bons paramètre en entrant init : 
    crc_cmd(3, (char*[]){"crc","set","1"});
    syncword_cmd(3, (char*[]){"syncword","set","AA"});
    lora_setup_cmd(4, (char*[]){"setup","125","11","5"});
    channel_cmd(3, (char*[]){"channel","set","869462500"});
    sx127x_set_preamble_length(&sx127x, 16);
Ces lignes simulent les commandes qu'on rentre au clavier.
