# ApI-Project-In-asp.net-core
Controller Methods


using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using ReminderbuzzAPI.Models;

namespace ReminderbuzzAPI.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ValuesController : ControllerBase
    {
        private readonly DBContext _Context;

        public ValuesController(DBContext context)
        {
            _Context = context;
        }


        [HttpPost("Register")]
        public ActionResult Register([FromBody]User user)
        {
            try
            {
                _Context.User.Add(user);
                _Context.SaveChanges();
            }
            catch (SqlException ex)
            {
                return StatusCode(500, new ResponseSqlException(ex.Message));
                throw;
            }
            catch (Exception ex)
            {
                return StatusCode(500, new ResponseException(ex.Message));
                throw;
            }
            return StatusCode(200, new Response("Register Successfull", user));
        }


        [HttpGet("Login")]
        public ActionResult Login(string username,string password)
        {
            var objuser = _Context.User.FirstOrDefault(e => e.User_Name == username && e.Password==password) ;

            if (objuser != null)
            {
                return StatusCode(200,new Response("Login Successfull", objuser));
            }
            else
            {
                return StatusCode(400, new ResponseNotFound());
            }

        }

        [HttpPost("AddTask")]
        public ActionResult AddTask([FromBody]Models.Task task)
        {
            try
            {
                _Context.Task.Add(task);
                _Context.SaveChanges();
            }
            catch (SqlException ex)
            {
                return StatusCode(500, new ResponseSqlException(ex.Message));
                throw;
            }
            catch (Exception ex)
            {
                return StatusCode(500, new ResponseException(ex.Message));
                throw;
            }
            return StatusCode(200, new Response("Task is Successfully Added", task));
        }

        [HttpPost("AddGroup")]
        public ActionResult AddGroup([FromBody]Group group)
        {
            try
            {
                _Context.Group.Add(group);
                _Context.SaveChanges();
            }
            catch (SqlException ex)
            {
                return StatusCode(500, new ResponseSqlException(ex.Message));
                throw;
            }
            catch(Exception ex)
            {
                return StatusCode(500, new ResponseException(ex.Message));
                throw;
            }
            return StatusCode(200, new Response("Group Successfully Added", group));
        }



        [HttpGet("TaskList")]

        public IEnumerable<Models.Task> TaskList()
        {
             return _Context.Task.ToList();
        }


        [HttpGet("GroupList")]

        public IEnumerable<Group> GroupList()
        {
            return _Context.Group.ToList();

        }
    }
}
