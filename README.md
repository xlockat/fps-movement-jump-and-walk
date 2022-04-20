# fps-movement-jump-and-walk
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class mousemovement : MonoBehaviour
{
    public float mouseSensitivity = 100f ;
    public Transform player;
    float xRotation = 0f;
    // Start is called before the first frame update
    void Start()
    {
       // cursor.visible= false;
        Cursor.lockState = CursorLockMode.Locked;
    }

    // Update is called once per frame
    void Update()
    {
        
        float mouseX= Input.GetAxis("Mouse Y" )* Time.deltaTime*mouseSensitivity;
        float mouseY= Input.GetAxis("Mouse X" )* Time.deltaTime*mouseSensitivity;

         xRotation -=mouseY;
         // to avoid over rotation
         xRotation = Mathf.Clamp(xRotation, -90f, 90f);
         // responsible for rotation in unity
         transform.localRotation = Quaternion.Euler( xRotation, 0f,0f);

         player.Rotate(Vector3.up*mouseX);
    }
}
// /Player movement script( it is a separate script)
public class playerMove : MonoBehaviour
{
    public CharacterController Controller;
    public float speed =12f;
    public float jump = 7f;
    public bool playerOnTheground = true;
    private Rigidbody playerRB;
    // Start is called before the first frame update
    void Start()
    {
        playerRB = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        // getting input
        float X = Input.GetAxis("Horizontal");
        float Z = Input.GetAxis("Vertical");
// we use transform instead of Vector 3 because we want to adjust the movement basing on where the character is facing.
        Vector3 move = transform.right * X + transform.forward * Z;
        Controller.Move (move *speed*Time.deltaTime);
        // jumping
        if (Input.GetKeyDown(KeyCode.Space)&& playerOnTheground)
        {
         playerRB.AddForce(Vector3.up * jump, ForceMode.Impulse);
         playerOnTheground =false;
        }                     

    }
}
