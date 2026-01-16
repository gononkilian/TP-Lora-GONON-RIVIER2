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


