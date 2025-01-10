# USI_I-NUCLEO-LRWAN1

The I-NUCLEO-LRWAN1 features the USI LoraWAN module WM-SG-SM-42 with preloaded AT-Command software, and can be controlled from an external host such as NUCLEO-L053 boards. You can also develop your own application based on the I-CUBE-LRWAN to control it without external host.
You just need to correct the ping definition and antenna switch logic to run the I-CUBE-LRWAN on the I-NUCLEO-LRWAN1. 

The following shows how to patch the ping definition and antenna control logic:

(1)	Download the I-CUBE-LRWAN 1.1.0 from the link below:

(2)	Download the IO definition header file of I-NUCLEO-LRWAN from the Github link below:
  https://github.com/USIWPModule/USILoRaModule/USI_I-NUCLEO-LRWAN1/blob/master/stm32l0xx_hw_conf.h.1.1.0

(3)	Unzip the en.i-cube_lrwan.zip, and replace stm32l0xx_hw_conf.h with the downloaded ‘stm32l0xx_hw_conf.h.1.1.0’ depended on the working project below:

  a.	For AT_Master project:
  en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Projects\Multi\Applications\LoRa\AT_Master\inc\stm32l0xx_hw_conf.h
  b.	For End_Node project::
  en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Projects\Multi\Applications\LoRa\End_Node\inc\stm32l0xx_hw_conf.h
  c.	For PingPong project:
  en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Projects\Multi\Applications\LoRa\PingPong\inc\stm32l0xx_hw_conf.h

(4)	Correct the antenna switch logic for I-NUCLEO-LRWAN1

// 1. Replace SX1272IoInit() in sx1272mb2das.c with the code below.

void SX1272IoInit( void )

{


  GPIO_InitTypeDef initStruct={0};
    
  initStruct.Mode = GPIO_MODE_IT_RISING;
  
  initStruct.Pull = GPIO_PULLDOWN;
  
  initStruct.Speed = GPIO_SPEED_HIGH;

  HW_GPIO_Init( RADIO_DIO_0_PORT, RADIO_DIO_0_PIN, &initStruct );
  
  HW_GPIO_Init( RADIO_DIO_1_PORT, RADIO_DIO_1_PIN, &initStruct );
  
  HW_GPIO_Init( RADIO_DIO_2_PORT, RADIO_DIO_2_PIN, &initStruct );
  
  HW_GPIO_Init( RADIO_DIO_3_PORT, RADIO_DIO_3_PIN, &initStruct );
	
  /* Initialize I-NUCLEO-LRWAN1 Antenna IO */
  
  initStruct.Mode =GPIO_MODE_OUTPUT_PP;
  
  initStruct.Pull = GPIO_NOPULL; 
  
  initStruct.Speed = GPIO_SPEED_HIGH;
    
  HW_GPIO_Init( RADIO_ANT_SWITCH_PORT, RADIO_ANT_SWITCH_PIN, &initStruct  ); 
  
  HW_GPIO_Init( RADIO_ANT2_SWITCH_PORT, RADIO_ANT2_SWITCH_PIN, &initStruct  ); 

}



// 2. Replace SX1272SetAntSw() in sx1272mb2das.c with the code below.

void SX1272SetAntSw( uint8_t opMode )

{


    switch( opMode )
    {
    
    case RFLR_OPMODE_TRANSMITTER:
    
	SX1272.RxTx = 1;
	
        /* Switch the antenna of I-NUCLEO-LRWAN1 in TX mode */
	
        HW_GPIO_Write( RADIO_ANT_SWITCH_PORT, RADIO_ANT_SWITCH_PIN, 1);
        
	HW_GPIO_Write( RADIO_ANT2_SWITCH_PORT, RADIO_ANT2_SWITCH_PIN, 0);
        
	break;
	
    case RFLR_OPMODE_RECEIVER:
    
    case RFLR_OPMODE_RECEIVER_SINGLE:
    
    case RFLR_OPMODE_CAD:
    
    default:
    
        SX1272.RxTx = 0;
	
        /* Switch the antenna of I-NUCLEO-LRWAN1 in RX mode */
	
        HW_GPIO_Write( RADIO_ANT_SWITCH_PORT, RADIO_ANT_SWITCH_PIN, 0);
	
        HW_GPIO_Write( RADIO_ANT2_SWITCH_PORT, RADIO_ANT2_SWITCH_PIN, 1);
	
        break;
	
    }
    
}


(5)	Patch is done at this step, you can make AT_Master, End_Node or PingPong project code for I-NUCLEO-LRWAN.

(6)	You can test the LoraWAN connectivity with Semtech sx1301 gateway starter-kit using End_Node project code, and just need to correct the configurations below:

  a.	in 'en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Projects\Multi\Applications\LoRa\End_Node\inc\Comissioning.h':
  
  // correct the definition below depended on your sx1301 gateway:
  
  OVER_THE_AIR_ACTIVATION
  
  LORAWAN_APPLICATION_EUI
  
  LORAWAN_APPLICATION_KEY
  
  LORAWAN_DEVICE_ADDRESS
  
  LORAWAN_NWKSKEY
  
  LORAWAN_APPSKEY
   
  
  b.	In 'en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Middlewares\Third_Party\Lora\Mac\region\RegionEU868.h'
  
  #define EU868_DUTY_CYCLE_ENABLED 0  // to enabling to update node data every 10 seconds to gateway
  
  c.	In 'en.i-cube_lrwan\STM32CubeExpansion_LRWAN_V1.1.0\Middlewares\Third_Party\Lora\Core\lora.c'
  
  #define USE_SEMTECH_DEFAULT_CHANNEL_LINEUP  1   // let the node uses DR3 on RX2


