# unity-class
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class character : MonoBehaviour
{
    private bool isInWater;
    private GameObject water;
    private float waterY;
    private const float floatageForce = 0;
    private const float density = 2;
    private const float g = 9.8f;
    private const float waterDrag = 2;

    private BoxCollider mCollider = null;

    public bool getIsInWater()
    {
        return isInWater;
    }
    public void setIsInWater(bool isInWater)
    {
        this.isInWater = isInWater;
    }

    private void Awake()
    {
        mCollider = GetComponent<BoxCollider>();
    }

    // Use this for initialization  
    void Start()
    {
        isInWater = false;
        water = GameObject.FindWithTag("water");
    }

    // Update is called once per frame  
    void Update()
    {

    }
    /// <summary>  
    /// This function is called every fixed framerate frame, if the MonoBehaviour is enabled.  
    /// </summary>  
    void FixedUpdate()
    {
        if (isInWater)
        {
            //calculate floatage  
            calFloatage();
            //change darg  
            GetComponent<Rigidbody>().drag = waterDrag;
        }
    }
    //calculate and add floatage force to the box.  
    private void calFloatage()
    {
        waterY = water.transform.position.y;

        var halfSizeY = mCollider.size.y * transform.localScale.y / 2.0f;
        var sizeX = mCollider.size.x * transform.localScale.x;
        var sizeZ = mCollider.size.z * transform.localScale.z;
        var objectY = transform.position.y + mCollider.center.y;

        if (waterY > (objectY- halfSizeY))
        {
            float h = waterY - (objectY - halfSizeY) > halfSizeY ? halfSizeY : waterY - (objectY - halfSizeY);
            float floatageForce = density * g * sizeX * sizeZ * h;
            GetComponent<Rigidbody>().AddForce(0, floatageForce, 0);
        }
    }
}
