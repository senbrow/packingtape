# packingtape
Generates serialization and deserialization code for existing C structs.

## Dependencies
[clang](https://github.com/llvm-mirror/clang/tree/master/bindings/python)

## Disclaimer
This tool is currently in the "quick weekend project" stage of development; it may eat your laundry.

## Example
Given the following ```game.h``` C header file:
```c
#include <stdint.h>

struct Player {
    uint8_t height;
    uint16_t weight;
    uint32_t position[2];
};

struct Team {
    struct Player players[3];
};

struct Game {
    uint32_t timeRemaining;
    struct Team teams[2];
};

```

Invoking this command
```
packingtape --preamble "#include <stdint.h>" --forward-declarations --header game.h
```

produces this 'header file' output:
```c
#include <stdint.h>

struct Player;
struct Team;
struct Game;

extern const size_t PLAYER_SERIALIZED_SIZE;
extern const size_t TEAM_SERIALIZED_SIZE;
extern const size_t GAME_SERIALIZED_SIZE;

size_t serialize_Player(const struct Player *source, uint8_t *outputBuffer, size_t outputBufferLength);
size_t serialize_Team(const struct Team *source, uint8_t *outputBuffer, size_t outputBufferLength);
size_t serialize_Game(const struct Game *source, uint8_t *outputBuffer, size_t outputBufferLength);

size_t deserialize_Player(const uint8_t *data, size_t dataLength, struct Player *output);
size_t deserialize_Team(const uint8_t *data, size_t dataLength, struct Team *output);
size_t deserialize_Game(const uint8_t *data, size_t dataLength, struct Game *output);

```

Similarly, invoking this command
```
packingtape --preamble $'#include <arpa/inet.h>\n#include <string.h>\n#include "game.h"' --header game.h
```

produces this 'implementation' output:
```c
#include <arpa/inet.h>
#include <string.h>
#include "game.h"

const size_t PLAYER_SERIALIZED_SIZE = 19;
const size_t TEAM_SERIALIZED_SIZE = 108;
const size_t GAME_SERIALIZED_SIZE = 148;

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t serialize_Player(const struct Player *source, uint8_t *outputBuffer, size_t outputBufferLength) {
    uint32_t temporary32;
    uint16_t temporary16;

    if(!outputBuffer) {
        return -1;
    }else if(outputBufferLength < PLAYER_SERIALIZED_SIZE) {
        return -2;
    }

    memcpy(outputBuffer, &(source->height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    return PLAYER_SERIALIZED_SIZE;
}

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t serialize_Team(const struct Team *source, uint8_t *outputBuffer, size_t outputBufferLength) {
    uint32_t temporary32;
    uint16_t temporary16;

    if(!outputBuffer) {
        return -1;
    }else if(outputBufferLength < TEAM_SERIALIZED_SIZE) {
        return -2;
    }

    memcpy(outputBuffer, &(source->players[0].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->players[0].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->players[0].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->players[0].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->players[1].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->players[1].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->players[1].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->players[1].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->players[2].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->players[2].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->players[2].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->players[2].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    return TEAM_SERIALIZED_SIZE;
}

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t serialize_Game(const struct Game *source, uint8_t *outputBuffer, size_t outputBufferLength) {
    uint32_t temporary32;
    uint16_t temporary16;

    if(!outputBuffer) {
        return -1;
    }else if(outputBufferLength < GAME_SERIALIZED_SIZE) {
        return -2;
    }

    temporary32 = htonl(source->timeRemaining);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[0].players[0].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[0].players[0].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[0].players[0].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[0].players[0].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[0].players[1].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[0].players[1].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[0].players[1].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[0].players[1].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[0].players[2].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[0].players[2].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[0].players[2].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[0].players[2].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[1].players[0].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[1].players[0].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[1].players[0].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[1].players[0].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[1].players[1].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[1].players[1].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[1].players[1].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[1].players[1].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    memcpy(outputBuffer, &(source->teams[1].players[2].height), sizeof(uint8_t));
    outputBuffer += sizeof(uint8_t);

    temporary16 = htons(source->teams[1].players[2].weight);
    memcpy(outputBuffer, &temporary16, sizeof(uint16_t));
    outputBuffer += sizeof(uint16_t);

    temporary32 = htonl(source->teams[1].players[2].position[0]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    temporary32 = htonl(source->teams[1].players[2].position[1]);
    memcpy(outputBuffer, &temporary32, sizeof(uint32_t));
    outputBuffer += sizeof(uint32_t);

    return GAME_SERIALIZED_SIZE;
}

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t deserialize_Player(const uint8_t *data, size_t dataLength, struct Player *output) {
   if(!data) {
       return -1;
   }else if(dataLength < PLAYER_SERIALIZED_SIZE) {
       return -2;
   }
    memcpy(&(output->height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->weight), data, sizeof(uint16_t));
    output->weight = ntohs(output->weight);
    data += sizeof(uint16_t);

    memcpy(&(output->position[0]), data, sizeof(uint32_t));
    output->position[0] = ntohl(output->position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->position[1]), data, sizeof(uint32_t));
    output->position[1] = ntohl(output->position[1]);
    data += sizeof(uint32_t);

    return PLAYER_SERIALIZED_SIZE;
}

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t deserialize_Team(const uint8_t *data, size_t dataLength, struct Team *output) {
   if(!data) {
       return -1;
   }else if(dataLength < TEAM_SERIALIZED_SIZE) {
       return -2;
   }
    memcpy(&(output->players[0].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->players[0].weight), data, sizeof(uint16_t));
    output->players[0].weight = ntohs(output->players[0].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->players[0].position[0]), data, sizeof(uint32_t));
    output->players[0].position[0] = ntohl(output->players[0].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->players[0].position[1]), data, sizeof(uint32_t));
    output->players[0].position[1] = ntohl(output->players[0].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->players[1].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->players[1].weight), data, sizeof(uint16_t));
    output->players[1].weight = ntohs(output->players[1].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->players[1].position[0]), data, sizeof(uint32_t));
    output->players[1].position[0] = ntohl(output->players[1].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->players[1].position[1]), data, sizeof(uint32_t));
    output->players[1].position[1] = ntohl(output->players[1].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->players[2].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->players[2].weight), data, sizeof(uint16_t));
    output->players[2].weight = ntohs(output->players[2].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->players[2].position[0]), data, sizeof(uint32_t));
    output->players[2].position[0] = ntohl(output->players[2].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->players[2].position[1]), data, sizeof(uint32_t));
    output->players[2].position[1] = ntohl(output->players[2].position[1]);
    data += sizeof(uint32_t);

    return TEAM_SERIALIZED_SIZE;
}

// **** AUTOMATICALLY GENERATED, DO NOT EDIT ****
size_t deserialize_Game(const uint8_t *data, size_t dataLength, struct Game *output) {
   if(!data) {
       return -1;
   }else if(dataLength < GAME_SERIALIZED_SIZE) {
       return -2;
   }
    memcpy(&(output->timeRemaining), data, sizeof(uint32_t));
    output->timeRemaining = ntohl(output->timeRemaining);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[0].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[0].players[0].weight), data, sizeof(uint16_t));
    output->teams[0].players[0].weight = ntohs(output->teams[0].players[0].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[0].players[0].position[0]), data, sizeof(uint32_t));
    output->teams[0].players[0].position[0] = ntohl(output->teams[0].players[0].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[0].position[1]), data, sizeof(uint32_t));
    output->teams[0].players[0].position[1] = ntohl(output->teams[0].players[0].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[1].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[0].players[1].weight), data, sizeof(uint16_t));
    output->teams[0].players[1].weight = ntohs(output->teams[0].players[1].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[0].players[1].position[0]), data, sizeof(uint32_t));
    output->teams[0].players[1].position[0] = ntohl(output->teams[0].players[1].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[1].position[1]), data, sizeof(uint32_t));
    output->teams[0].players[1].position[1] = ntohl(output->teams[0].players[1].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[2].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[0].players[2].weight), data, sizeof(uint16_t));
    output->teams[0].players[2].weight = ntohs(output->teams[0].players[2].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[0].players[2].position[0]), data, sizeof(uint32_t));
    output->teams[0].players[2].position[0] = ntohl(output->teams[0].players[2].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[0].players[2].position[1]), data, sizeof(uint32_t));
    output->teams[0].players[2].position[1] = ntohl(output->teams[0].players[2].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[0].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[1].players[0].weight), data, sizeof(uint16_t));
    output->teams[1].players[0].weight = ntohs(output->teams[1].players[0].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[1].players[0].position[0]), data, sizeof(uint32_t));
    output->teams[1].players[0].position[0] = ntohl(output->teams[1].players[0].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[0].position[1]), data, sizeof(uint32_t));
    output->teams[1].players[0].position[1] = ntohl(output->teams[1].players[0].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[1].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[1].players[1].weight), data, sizeof(uint16_t));
    output->teams[1].players[1].weight = ntohs(output->teams[1].players[1].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[1].players[1].position[0]), data, sizeof(uint32_t));
    output->teams[1].players[1].position[0] = ntohl(output->teams[1].players[1].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[1].position[1]), data, sizeof(uint32_t));
    output->teams[1].players[1].position[1] = ntohl(output->teams[1].players[1].position[1]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[2].height), data, sizeof(uint8_t));
    data += sizeof(uint8_t);

    memcpy(&(output->teams[1].players[2].weight), data, sizeof(uint16_t));
    output->teams[1].players[2].weight = ntohs(output->teams[1].players[2].weight);
    data += sizeof(uint16_t);

    memcpy(&(output->teams[1].players[2].position[0]), data, sizeof(uint32_t));
    output->teams[1].players[2].position[0] = ntohl(output->teams[1].players[2].position[0]);
    data += sizeof(uint32_t);

    memcpy(&(output->teams[1].players[2].position[1]), data, sizeof(uint32_t));
    output->teams[1].players[2].position[1] = ntohl(output->teams[1].players[2].position[1]);
    data += sizeof(uint32_t);

    return GAME_SERIALIZED_SIZE;
}

```
