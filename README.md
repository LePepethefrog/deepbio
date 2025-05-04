# deepbio

## Release Info

Full Release of both Deep F1 and Deep F2, no reputation nor daily quest were added as they dont fit the server vibes

## Source modifications to the emulator?

Deep F2 Monsters ATK go beyond the 65535 unsigned short limit  so ...
status.hpp was changed from 
/** Basic damage info of a weapon
* Required because players have two of these, one in status_data
* and another for their left hand weapon. */
struct weapon_atk {
	unsigned short atk, atk2;
	unsigned short range;
	unsigned char ele;

to

/** Basic damage info of a weapon
* Required because players have two of these, one in status_data
* and another for their left hand weapon. */
struct weapon_atk {
	uint32 atk, atk2;
	unsigned short range;
	unsigned char ele;

script.cpp 

    case UMOB_ATKMIN: md->base_status->rhw.atk = (unsigned short)value; calc_status = true; break;
		case UMOB_ATKMAX: md->base_status->rhw.atk2 = (unsigned short)value; calc_status = true; break;
  to
		case UMOB_ATKMIN: md->base_status->rhw.atk = (uint32)value; calc_status = true; break;
		case UMOB_ATKMAX: md->base_status->rhw.atk2 = (uint32)value; calc_status = true; break;

mob.cpp

	if (this->nodeExists(node, "Attack")) {
		uint16 atk;

		if (!this->asUInt16(node, "Attack", atk))
			return 0;

		mob->status.rhw.atk = atk;
	}
	
	if (this->nodeExists(node, "Attack2")) {
		uint16 atk;

		if (!this->asUInt16(node, "Attack2", atk))
			return 0;

to

	if (this->nodeExists(node, "Attack")) {
		uint32 atk;

		if (!this->asUInt32(node, "Attack", atk))
			return 0;

		mob->status.rhw.atk = atk;
	}
	
	if (this->nodeExists(node, "Attack2")) {
		uint32 atk;

		if (!this->asUInt32(node, "Attack2", atk))
			return 0;
   
Before and After
   ![image](https://github.com/user-attachments/assets/7431632c-7961-4667-bd79-7a888960d92a)
   ![image](https://github.com/user-attachments/assets/dae12ba8-b445-481e-934b-ae27d2e21f2f)

THIS HOWEVER GOES BEYOND MY LEVEL OF KNOWLEDGE so I cant speak about the long term effects of this change nor the necessity of every modification I made, I basically updated right hand weapon (rhw) to be an uint32 instead of an unsigned short or an uint16 and made changes to monster coding accordingly to that, it worked well and the damage was the expected damage from the monsters that way.

## About the Time Dim Circlets enchanter

Officialy as is explain in the comments of the npc the enchant uses the new UI, I grabbed the OG nightmare biolabs script (the only one thats available on rathena) and modified it with some help from chat gpt in order to use it with the time dim circlets, the rates are the official ones that are somewhat terrible and you can find them on the official kro website or hazy forest




https://github.com/user-attachments/assets/8249255a-de9e-484f-af14-4424815022db



